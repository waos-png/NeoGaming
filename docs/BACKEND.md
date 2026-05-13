# Backend Context

## Framework and Version

- Framework: Spring Boot `4.0.5`
- Language/runtime: Java 21
- Build tool: Maven Wrapper
- Persistence: Spring Data JPA with PostgreSQL
- Migrations: Flyway
- Security: Spring Security with stateless JWT bearer tokens
- API documentation: Springdoc OpenAPI `3.0.0`

## Folder Structure

```text
backend/
|-- compose.yaml
|-- Dockerfile
|-- pom.xml
|-- postman/
|-- src/
|   |-- main/
|   |   |-- java/com/neogamin/proyecto_formativo/
|   |   |   |-- admin/
|   |   |   |   |-- api/
|   |   |   |   |-- aplicacion/
|   |   |   |   |-- dominio/
|   |   |   |   |-- infraestructura/
|   |   |   |-- analitica/
|   |   |   |-- carrito/
|   |   |   |-- catalogo/
|   |   |   |-- checkout/
|   |   |   |-- compartido/
|   |   |   |   |-- api/
|   |   |   |   |-- aplicacion/
|   |   |   |   |-- dominio/
|   |   |   |   |-- infraestructura/
|   |   |   |   |-- seguridad/
|   |   |   |-- facturacion/
|   |   |   |-- interaccion/
|   |   |   |-- inventario/
|   |   |   |-- notificacion/
|   |   |   |-- pago/
|   |   |   |-- pedido/
|   |   |   |-- resena/
|   |   |   |-- usuario/
|   |   |   |-- ProyectoFormativoApplication.java
|   |   |-- resources/
|   |       |-- application.yml
|   |       |-- db/migration/
|   |-- test/
```

## API Endpoints

Security note: `ConfiguracionSeguridad` permits `/api/auth/**`, `/actuator/health`, `/actuator/info`, Swagger/OpenAPI endpoints, and `GET /api/catalogo/productos/**`. Every other route requires authentication unless a controller method applies a stricter role rule.

| Method | Path | Auth | Expected input | Expected output | Description |
| --- | --- | --- | --- | --- | --- |
| `GET` | `/actuator/health` | Public | None | Actuator health payload | Healthcheck. |
| `GET` | `/actuator/info` | Public | None | Actuator info payload | Service info. |
| `GET` | `/swagger-ui/**` | Public | None | Swagger UI | Interactive API docs. |
| `GET` | `/v3/api-docs/**` | Public | None | OpenAPI JSON | API schema. |
| `POST` | `/api/auth/registro` | Public | `RegistroUsuarioRequest {nombre, email, password, telefono}` | `UsuarioResponse` | Register a customer user. |
| `POST` | `/api/auth/login` | Public | `LoginRequest {email, password}` | `LoginResponse {token, usuarioId, nombre, email, rol}` | Authenticate and issue JWT. |
| `POST` | `/api/auth/logout` | Public route, token optional | `Authorization` header | `204 No Content` | Revoke active session when a JWT is provided. |
| `GET` | `/api/usuarios/me` | Authenticated | None | `UsuarioResponse` | Current user summary. |
| `GET` | `/api/usuarios/perfil` | Authenticated | None | `PerfilUsuarioResponse` | Current profile details. |
| `PUT` | `/api/usuarios/perfil` | Authenticated | `ActualizarPerfilUsuarioRequest {nombre, email, telefono, sobreMi, fotoPerfilUrl, prefiereNoticias, prefiereOfertas}` | `PerfilUsuarioResponse` | Update current profile. |
| `GET` | `/api/usuarios/direcciones` | Authenticated | None | `List<DireccionResponse>` | List current user's addresses. |
| `GET` | `/api/usuarios/direcciones/{idDireccion}` | Authenticated | Path `idDireccion` | `DireccionResponse` | Get one current-user address. |
| `POST` | `/api/usuarios/direcciones` | Authenticated | `GuardarDireccionRequest {tipo, esPrincipal, pais, departamento, ciudad, comuna, codigoPostal, calle, numero, referencia}` | `DireccionResponse` | Create address. |
| `PUT` | `/api/usuarios/direcciones/{idDireccion}` | Authenticated | Path `idDireccion`, `GuardarDireccionRequest` | `DireccionResponse` | Update address. |
| `PATCH` | `/api/usuarios/direcciones/{idDireccion}/principal` | Authenticated | Path `idDireccion` | `DireccionResponse` | Mark address as default. |
| `DELETE` | `/api/usuarios/direcciones/{idDireccion}` | Authenticated | Path `idDireccion` | `204 No Content` | Delete address. |
| `POST` | `/api/usuarios/convertir-vendedor` | Authenticated | `ConvertirVendedorRequest {nombreCompletoORazonSocial, tipoDocumento, numeroDocumento, pais, telefono, correo, nombreComercial, aceptaTerminos, datosPago}` | `VendedorResponse` | Convert current user to seller. |
| `GET` | `/api/catalogo/productos` | Public | `FiltroProductoRequest` query params | `Page<ProductoListadoResponse>` | List products with filters and pagination. |
| `GET` | `/api/catalogo/productos/{idProducto}` | Public | Path `idProducto` | `ProductoDetalleResponse` | Product detail by id. |
| `GET` | `/api/catalogo/productos/slug/{slug}` | Public | Path `slug` | `ProductoDetalleResponse` | Product detail by slug. |
| `GET` | `/api/catalogo/productos/buscar-natural` | Public | `BusquedaNaturalProductoRequest {texto, soloDisponibles, page, size}` query params | `Page<ProductoBusquedaResponse>` | Natural-language product search. |
| `POST` | `/api/catalogo/productos` | Authenticated | `CrearProductoRequest {categoriaId, sku, slug, nombre, descripcion, moneda, precioLista, stockFisico, condicion}` | `ProductoResponse` | Create product. |
| `PUT` | `/api/catalogo/productos/{idProducto}` | Authenticated | Path `idProducto`, `ActualizarProductoRequest` | `ProductoResponse` | Update product. |
| `PATCH` | `/api/catalogo/productos/{idProducto}/precio` | Authenticated | `ActualizarPrecioProductoRequest {nuevoPrecio, motivo}` | `ProductoResponse` | Update product price. |
| `PATCH` | `/api/catalogo/productos/{idProducto}/stock` | Authenticated | `ActualizarStockProductoRequest {stockFisico, motivo}` | `ProductoResponse` | Update product stock. |
| `POST` | `/api/catalogo/productos/{idProducto}/imagenes` | Authenticated | `AgregarProductoImagenRequest {urlImagen, altText, orden, principal}` | `ProductoImagenResponse` | Add product image. |
| `PATCH` | `/api/catalogo/productos/{idProducto}/imagenes/{idImagen}/principal` | Authenticated | Path `idProducto`, `idImagen` | `ProductoImagenResponse` | Set main product image. |
| `POST` | `/api/catalogo/categorias` | Authenticated | `CrearCategoriaRequest {categoriaPadreId, nombre, slug, descripcion}` | `CategoriaResponse` | Create category. |
| `GET` | `/api/catalogo/categorias/arbol` | Authenticated | None | `List<CategoriaArbolResponse>` | Category tree. |
| `GET` | `/api/catalogo/categorias/{idCategoria}/productos` | Authenticated | Path `idCategoria`, `FiltroProductoRequest` query params | `Page<ProductoListadoResponse>` | Products by category. |
| `GET` | `/api/catalogo/ofertas/activas` | Authenticated | None | `List<OfertaActivaResponse>` | Active offers. |
| `POST` | `/api/catalogo/ofertas` | Authenticated | `CrearOfertaRequest {productoId, titulo, descripcion, porcentajeDesc, precioOferta, fechaInicio, fechaFin, estado}` | `OfertaResponse` | Create offer. |
| `GET` | `/api/carrito` | Authenticated | None | `CarritoResponse` | Current user's cart. |
| `POST` | `/api/carrito/items` | Authenticated | `AgregarProductoCarritoRequest {productoId, cantidad}` | `CarritoResponse` | Add product to cart. |
| `PATCH` | `/api/carrito/items/{idItem}` | Authenticated | Path `idItem`, `ActualizarCantidadCarritoRequest {cantidad}` | `CarritoResponse` | Update cart quantity. |
| `DELETE` | `/api/carrito/items/{idItem}` | Authenticated | Path `idItem` | `CarritoResponse` | Remove cart item. |
| `DELETE` | `/api/carrito` | Authenticated | None | `CarritoResponse` | Empty cart. |
| `POST` | `/api/carrito/convertir-a-pedido` | Authenticated | None | `PedidoResponse` | Convert cart to order. |
| `GET` | `/api/pedidos/mis-pedidos` | Authenticated | `FiltroMisPedidosRequest {estado, page, size}` query params | `Page<PedidoListadoResponse>` | Current user's order list. |
| `POST` | `/api/pedidos` | Authenticated | Optional `CrearPedidoRequest {moneda, direccionEnvioId, direccionFacturaId}` | `PedidoResponse` | Create order. |
| `POST` | `/api/pedidos/{pedidoId}/items` | Authenticated | `AgregarItemPedidoRequest {productoId, cantidad, descuentoUnitario, impuestoUnitario}` | `PedidoResponse` | Add item to order. |
| `POST` | `/api/pedidos/{pedidoId}/recalcular` | Authenticated | Path `pedidoId` | `PedidoResponse` | Recalculate order totals. |
| `POST` | `/api/pedidos/{pedidoId}/checkout` | Authenticated | `CheckoutRequest {proveedorPago, referenciaExterna, idempotencyKey, tipoPago}` | `CheckoutResponse` | Checkout an existing order. |
| `POST` | `/api/pedidos/{pedidoId}/cancelar` | Authenticated | Path `pedidoId` | `PedidoResponse` | Cancel order. |
| `GET` | `/api/pedidos/{pedidoId}` | Authenticated | Path `pedidoId` | `PedidoResponse` | Get order detail. |
| `POST` | `/api/checkout` | Authenticated | None | `IniciarCheckoutResponse` | Start checkout from cart. |
| `POST` | `/api/checkout/envio` | Authenticated | `GuardarEnvioRequest {pedidoId, direccionEnvioId, direccionFacturaId, direccionEnvio, direccionFactura, mismaDireccionFacturacion}` | `IniciarCheckoutResponse` | Save shipping and billing data. |
| `POST` | `/api/checkout/pago` | Authenticated | `ProcesarPagoRequest {pedidoId, metodoPago, simularFallo}` | `ProcesarPagoResponse` | Process simulated payment. |
| `GET` | `/api/checkout/confirmacion/{numeroPedido}` | Authenticated | Path `numeroPedido` | `ConfirmacionPedidoResponse` | Order confirmation. |
| `GET` | `/api/pagos/{pagoId}` | Authenticated | Path `pagoId` | `PagoResponse` | Payment detail. |
| `POST` | `/api/pagos/{pagoId}/aprobar` | `ADMIN` or `VENDEDOR` | Path `pagoId` | `PagoResponse` | Approve payment. |
| `POST` | `/api/pagos/{pagoId}/rechazar` | `ADMIN` or `VENDEDOR` | Path `pagoId` | `PagoResponse` | Reject payment. |
| `GET` | `/api/facturas/pedido/{pedidoId}` | Authenticated | Path `pedidoId` | `FacturaResponse` | Invoice by order. |
| `POST` | `/api/resenas` | Authenticated | `CrearOActualizarResenaRequest {productoId, calificacion, comentario}` | `ResenaProductoResponse` | Create/update review. |
| `GET` | `/api/resenas/productos/{productoId}` | Authenticated | Path `productoId` | `List<ResenaProductoResponse>` | Product reviews. |
| `GET` | `/api/resenas/productos/{productoId}/resumen` | Authenticated | Path `productoId` | `ResumenCalificacionProductoResponse` | Product rating summary. |
| `DELETE` | `/api/resenas/{resenaId}` | Authenticated | Path `resenaId` | `204 No Content` | Soft-delete review. |
| `POST` | `/api/interacciones/productos/{productoId}/like` | Authenticated | Path `productoId` | `EstadoInteraccionResponse` | Toggle product like. |
| `POST` | `/api/interacciones/productos/{productoId}/wishlist` | Authenticated | Path `productoId` | `EstadoInteraccionResponse` | Toggle wishlist state. |
| `GET` | `/api/interaccion/wishlist` | Authenticated | None | `List<WishlistProductoResponse>` | Current user's wishlist. |
| `PATCH` | `/api/inventario/productos/{idProducto}/stock` | Authenticated | `AjustarStockProductoRequest {stockFisico, motivo}` | `StockProductoResponse` | Adjust inventory stock. |
| `GET` | `/api/admin/usuarios` | `ADMIN` | Query `rol`, `activo`, `q`, `page`, `size` | `Page<UsuarioAdminResponse>` | Admin user list. |
| `GET` | `/api/admin/usuarios/{id}` | `ADMIN` | Path `id` | `UsuarioAdminResponse` | Admin user detail. |
| `PATCH` | `/api/admin/usuarios/{id}/rol` | `ADMIN` | `CambiarRolRequest {rol}` | `UsuarioAdminResponse` | Change user role. |
| `PATCH` | `/api/admin/usuarios/{id}/estado` | `ADMIN` | Optional `CambiarEstadoUsuarioRequest {activo}` | `UsuarioAdminResponse` | Change user active state. |
| `DELETE` | `/api/admin/usuarios/{id}` | `ADMIN` | Path `id` | `204 No Content` | Delete/deactivate user. |
| `GET` | `/api/admin/productos` | `ADMIN` | Query `q`, `estado`, `categoriaId`, `page`, `size` | `Page<ProductoListadoResponse>` | Admin product list. |
| `PATCH` | `/api/admin/productos/{id}/aprobar` | `ADMIN` | Path `id` | `ProductoResponse` | Approve product. |
| `PATCH` | `/api/admin/productos/{id}/rechazar` | `ADMIN` | `RechazarProductoRequest {motivo}` | `ProductoResponse` | Reject product. |
| `GET` | `/api/admin/pedidos` | `ADMIN` | Query `estado`, `desde`, `hasta`, `page`, `size` | `Page<PedidoListadoResponse>` | Admin order list. |
| `GET` | `/api/admin/pedidos/{id}` | `ADMIN` | Path `id` | `PedidoResponse` | Admin order detail. |
| `PATCH` | `/api/admin/pedidos/{id}/estado` | `ADMIN` | `CambiarEstadoPedidoRequest {estado}` | `PedidoResponse` | Change order state. |
| `GET` | `/api/admin/vendedores` | `ADMIN` | Query `activo`, `page`, `size` | `Page<VendedorAdminResponse>` | Admin seller list. |
| `PATCH` | `/api/admin/vendedores/{id}/aprobar` | `ADMIN` | Path `id` | `VendedorAdminResponse` | Approve seller. |
| `PATCH` | `/api/admin/vendedores/{id}/suspender` | `ADMIN` | `SuspenderVendedorRequest {motivo}` | `VendedorAdminResponse` | Suspend seller. |
| `GET` | `/api/admin/configuracion` | `ADMIN` | None | `ConfiguracionPlataformaResponse` | Read platform configuration. |
| `PUT` | `/api/admin/configuracion` | `ADMIN` | `ConfiguracionPlataformaRequest` | `ConfiguracionPlataformaResponse` | Update platform configuration. |
| `GET` | `/api/analitica/admin/resumen` | Authenticated | None | `ResumenAdminResponse` | Admin analytics summary. |
| `GET` | `/api/analitica/admin/ventas-por-periodo` | Authenticated | Query `periodo` | `List<VentaPeriodoResponse>` | Admin sales by period. |
| `GET` | `/api/analitica/admin/top-vendedores` | Authenticated | None | `List<TopVendedorResponse>` | Top sellers. |
| `GET` | `/api/analitica/admin/top-productos` | Authenticated | None | `List<TopProductoResponse>` | Top products. |
| `GET` | `/api/analitica/admin/ventas-por-categoria` | Authenticated | None | `List<VentaCategoriaResponse>` | Sales by category. |
| `GET` | `/api/analitica/admin/metodos-pago` | Authenticated | None | `List<MetodoPagoResponse>` | Payment-method analytics. |
| `GET` | `/api/analitica/admin/pedidos-por-estado` | Authenticated | None | `List<PedidoEstadoResponse>` | Order counts by state. |
| `GET` | `/api/analitica/vendedor/resumen` | Authenticated | None | `ResumenVendedorResponse` | Seller analytics summary. |
| `GET` | `/api/analitica/vendedor/ventas-por-periodo` | Authenticated | Query `periodo` | `List<VentaPeriodoResponse>` | Seller sales by period. |
| `GET` | `/api/analitica/vendedor/productos-mas-vendidos` | Authenticated | None | `List<TopProductoResponse>` | Seller top products. |
| `GET` | `/api/analitica/vendedor/pedidos-por-estado` | Authenticated | None | `List<PedidoEstadoResponse>` | Seller order counts by state. |
| `GET` | `/api/analitica/vendedor/stock-bajo` | Authenticated | None | `List<StockBajoResponse>` | Low-stock seller products. |

## Database Schema and Models

Database engine: PostgreSQL. Schema management: Flyway migrations under `src/main/resources/db/migration`.

| Table | Main fields | Relationships |
| --- | --- | --- |
| `usuario` | `id_usuario`, `nombre`, `email`, `password_hash`, `telefono`, `numero_documento`, `rol`, `estado`, `sobre_mi`, `foto_perfil_url`, `prefiere_noticias`, `prefiere_ofertas`, `deleted_at`, timestamps | Parent for sessions, addresses, products as seller in the original product FK, orders, payments, reviews, likes, wishlist, seller profile. |
| `sesion` | `id_sesion`, `fk_usuario`, `token_hash`, `ip_origen`, `user_agent`, `activa`, `expira_en`, `creada_en`, `revocada_en` | Many sessions belong to one `usuario`; JWT validity is checked against active sessions. |
| `direccion` | `id_direccion`, `fk_usuario`, `tipo`, `es_principal`, `pais`, `departamento`, `ciudad`, `comuna`, `codigo_postal`, `calle`, `numero`, `referencia`, `estado`, `deleted_at`, timestamps | Many addresses belong to one `usuario`; orders reference shipping and billing addresses. |
| `categoria` | `id_categoria`, `fk_categoria_padre`, `nombre`, `slug`, `descripcion`, `estado`, `deleted_at`, timestamps | Self-referencing category tree; products belong to categories. |
| `producto` | `id_producto`, `fk_categoria`, `fk_vendedor`, `sku`, `slug`, `nombre`, `descripcion`, `moneda`, `precio_lista`, `precio_vigente_cache`, `stock_fisico`, `stock_reservado`, `needs_recalc`, `condicion`, `estado`, `deleted_at`, timestamps | Belongs to `categoria` and seller user; has images, offers, order lines, stock movements, reviews, likes, wishlist entries, price history. |
| `producto_imagen` | `id_imagen`, `fk_producto`, `url_imagen`, `alt_text`, `orden`, `es_principal`, `deleted_at`, timestamps | Many images belong to one `producto`. |
| `oferta` | `id_oferta`, `fk_producto`, `titulo`, `descripcion`, `porcentaje_desc`, `precio_oferta`, `fecha_inicio`, `fecha_fin`, `estado`, timestamps | Many offers belong to one `producto`. |
| `pedido` | `id_pedido`, `fk_usuario`, `moneda`, `fk_direccion_envio`, `fk_direccion_factura`, `numero_pedido`, address snapshots, totals, `needs_recalc`, `estado`, order dates, timestamps | Belongs to `usuario`; references addresses; has order details, payments, invoice, stock movements, reviews. |
| `pedido_detalle` | `id_detalle`, `fk_pedido`, `fk_producto`, product snapshot fields, `cantidad`, unit pricing, line totals, timestamps | Many details belong to one `pedido`; each detail references one `producto`; unique by order/product. |
| `producto_stock_movimiento` | `id_movimiento`, `fk_producto`, `fk_pedido`, `tipo_movimiento`, `cantidad`, previous/new physical and reserved stock, `motivo`, `fecha_movimiento` | Tracks inventory changes for products and optionally orders. |
| `pago` | `id_pago`, `fk_pedido`, `fk_usuario`, `proveedor_pago`, `referencia_interna`, `referencia_externa`, `idempotency_key`, `monto`, `moneda`, `tipo_pago`, `estado`, `payload_respuesta`, `fecha_evento`, timestamps | Payments belong to an order and user; invoices may reference payment. |
| `factura` | `id_factura`, `fk_pedido`, `fk_pago`, `numero_factura`, `moneda`, subtotal/discount/tax/shipping/total fields, `metodo_pago`, `fecha_emision`, `estado_factura`, timestamps | One invoice per order; optional reference to payment. |
| `resena` | `id_resena`, `fk_usuario`, `fk_producto`, `fk_pedido`, `compra_verificada`, `calificacion`, `comentario`, `fecha`, `deleted_at`, timestamps | One review per user/product; may reference verified purchase order. |
| `producto_like` | `id_like`, `fk_usuario`, `fk_producto`, `fecha_like` | Unique like per user/product. |
| `producto_deseado` | `id_deseado`, `fk_usuario`, `fk_producto`, `fecha_agregado` | Unique wishlist entry per user/product. |
| `auditoria_log` | `id_auditoria`, `tabla`, `operacion`, `id_registro`, `datos_anteriores`, `datos_nuevos`, `fecha_evento`, `usuario_db`, `app_user_id`, `observacion` | Audit log table; no explicit FK in migration. |
| `vendedor` | `id_vendedor`, `fk_usuario`, `nombre_completo_o_razon_social`, `tipo_documento`, `numero_documento`, `pais`, `telefono`, `correo`, `nombre_comercial`, `acepta_terminos`, timestamps | One seller profile per user. |
| `datos_pago_vendedor` | `id_datos_pago`, `fk_vendedor`, `tipo_cuenta`, `numero_cuenta`, `banco`, `titular_cuenta`, timestamps | One payment-data row per seller. |
| `carrito` | `id_carrito`, `fk_usuario`, `estado`, timestamps | One active cart per user via partial unique index. |
| `carrito_item` | `id_carrito_item`, `fk_carrito`, `fk_producto`, `cantidad`, timestamps | Many cart items belong to one cart; unique by cart/product. |
| `moneda_referencia` | `codigo`, `nombre`, `simbolo`, `activa` | Currency lookup table. |
| `producto_precio_historial` | `id_historial`, `fk_producto`, `moneda`, `precio_anterior`, `precio_nuevo`, `fecha_cambio`, `fk_usuario_cambio`, `motivo` | Product price audit trail; optional user reference. |
| `configuracion_plataforma` | `id`, `comision_vendedor`, `pedido_minimo`, `envio_gratis_desde`, `mantenimiento_activo`, `registro_abierto`, `vendedores_requieren_aprobacion` | Singleton platform settings row. |

Additional database objects:

- `pg_trgm` extension is enabled.
- Trigger function `trg_fn_validar_resena_verificada` validates verified reviews.
- Cart tables have `updated_at` trigger handling.
- Several indexes enforce uniqueness and common lookup paths.

## Authentication and Authorization

- Authentication uses stateless JWT bearer tokens.
- Login creates a revocable session row and returns a JWT with `uid`, `rol`, and `sid` claims.
- `JwtAuthenticationFilter` validates the token signature, expiration, subject, and active session hash before populating Spring Security context.
- Logout revokes the active session associated with the provided bearer token.
- Passwords are encoded with BCrypt.
- `SecurityFilterChain` disables CSRF, sets session policy to stateless, configures CORS, and applies route authorization.
- Method security is enabled through `@EnableMethodSecurity`.
- `/api/admin/**` requires role `ADMIN` at both matcher and controller levels.
- Payment approval/rejection requires `ADMIN` or `VENDEDOR`.
- Public endpoints are limited to auth, health/info, Swagger/OpenAPI, and GET product catalog endpoints.

## External Services and Integrations

| Integration | Evidence | Purpose |
| --- | --- | --- |
| PostgreSQL | `compose.yaml`, JDBC config, PostgreSQL driver | Primary relational database. |
| Flyway | `spring.flyway`, `db/migration`, `FlywayConfiguracion` | Database schema migrations. |
| SMTP email | Spring Mail dependency, `SmtpNotificadorCorreo`, mail properties | Transactional emails for registration, login, orders, payments, and invoices. |
| Springdoc OpenAPI / Swagger | `OpenApiConfiguracion`, dependency | API documentation and grouped OpenAPI specs. |
| Spring Actuator | dependency and `management` config | Health/info operational endpoints. |
| Payment provider | _Not detected_ | Payments are simulated/managed internally through payment services and status endpoints. |
| File/object storage | _Not detected_ | Product/profile images are stored as URL strings only. |

## Backend Environment Variables

| Key | Purpose |
| --- | --- |
| `PORT` | HTTP server port. |
| `DB_URL` | PostgreSQL JDBC URL. |
| `DB_USERNAME` | PostgreSQL username. |
| `DB_PASSWORD` | PostgreSQL password. |
| `MAIL_HOST` | SMTP host. |
| `MAIL_PORT` | SMTP port. |
| `MAIL_USERNAME` | SMTP username. |
| `MAIL_PASSWORD` | SMTP password. |
| `MAIL_SMTP_AUTH` | SMTP auth toggle. |
| `MAIL_SMTP_STARTTLS_ENABLE` | SMTP STARTTLS enable toggle. |
| `MAIL_SMTP_STARTTLS_REQUIRED` | SMTP STARTTLS required toggle. |
| `MAIL_SMTP_SSL_ENABLE` | SMTP SSL toggle. |
| `MAIL_SMTP_CONNECTION_TIMEOUT` | SMTP connection timeout. |
| `MAIL_SMTP_TIMEOUT` | SMTP timeout. |
| `MAIL_SMTP_WRITE_TIMEOUT` | SMTP write timeout. |
| `SPRINGDOC_API_DOCS_ENABLED` | Enable/disable OpenAPI docs. |
| `SPRINGDOC_SWAGGER_UI_ENABLED` | Enable/disable Swagger UI. |
| `APP_CORS_ORIGIN_1` | First allowed CORS origin. |
| `APP_CORS_ORIGIN_2` | Second allowed CORS origin. |
| `JWT_SECRET` | JWT signing secret. |
| `JWT_EXPIRATION_MINUTES` | JWT expiration in minutes. |
| `APP_NOTIFICACION_EMAIL_HABILITADO` | Enable/disable transactional email sending. |
| `APP_NOTIFICACION_EMAIL_REMITENTE` | Transactional email sender address. |

## Important Patterns and Conventions

- The backend is organized as a modular monolith by domain: `admin`, `catalogo`, `usuario`, `pedido`, `checkout`, `pago`, and related modules.
- Most modules follow `api`, `api/dto`, `aplicacion`, `dominio`, and `infraestructura` package boundaries.
- Controllers are thin and delegate behavior to services.
- DTOs are mostly Java records with Jakarta Bean Validation annotations.
- Persistence uses JPA entities and Spring Data repositories.
- Flyway migrations are the source of truth for schema creation and evolution.
- REST responses commonly use `ResponseEntity`.
- Pagination uses Spring Data `Page<T>` with `page` and `size` query parameters.
- Security helpers resolve the current user from Spring Security context.
- Custom application exceptions live in `compartido/aplicacion`; API error handling lives in `GlobalExceptionHandler`.
- OpenAPI grouping separates user, catalog, order, inventory, payment, invoice, review, and interaction docs.
- Soft deletion appears through `deleted_at` fields on several tables.
- The codebase uses Spanish domain naming for packages, DTOs, and database columns.
