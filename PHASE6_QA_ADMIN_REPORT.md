# NeoGaming Phase 6 - QA + Admin Endpoints

Fecha: 2026-05-12

## Track A - Step 0

- `frontend/src/environments/environment.ts` y `environment.development.ts` apuntan a `http://localhost:8080/api`; no hay URL Railway configurada en esos archivos.
- `ng serve` y `ng serve --configuration=development` sirven el frontend local, pero hoy ambos quedan contra localhost por esos environments.
- CORS en `ConfiguracionSeguridad` toma `app.cors.allowed-origins`; default: `http://localhost:4200` y `http://localhost:4000`.
- El backend usa prefijo `/api`.

## Track A - Step 1 Audit

| Servicio | Method | Frontend URL | Backend URL | Payload match | Response match | Status |
|---|---|---|---|---|---|---|
| auth.api.ts | login | `/auth/login` | `/api/auth/login` | OK | OK | OK |
| auth.api.ts | register | `/auth/registro` | `/api/auth/registro` | OK | OK | OK |
| auth.api.ts | me | `/usuarios/me` | `/api/usuarios/me` | OK | OK | OK |
| auth.api.ts | logout | `/auth/logout` | `/api/auth/logout` | OK | OK | OK |
| catalog.api.ts | getProducts | `/catalogo/productos` | `/api/catalogo/productos` | fixed params | fixed TS DTO | OK |
| catalog.api.ts | search | `/catalogo/productos/buscar-natural` | `/api/catalogo/productos/buscar-natural` | fixed params | fixed TS DTO | OK |
| catalog.api.ts | getCategories | `/catalogo/categorias/arbol` | `/api/catalogo/categorias/arbol` | OK | OK | OK |
| catalog.api.ts | getProductBySlug | `/catalogo/productos/slug/{slug}` | `/api/catalogo/productos/slug/{slug}` | OK | fixed TS DTO | OK |
| catalog.api.ts | getProductById | `/catalogo/productos/{id}` | `/api/catalogo/productos/{id}` | OK | fixed TS DTO | OK |
| catalog.api.ts | getRelatedProducts | `/catalogo/productos/{id}/relacionados` | missing | N/A | N/A | MISSING, left throwError TODO |
| cart.api.ts | getCart | `/carrito` | `/api/carrito` | OK | OK | OK |
| cart.api.ts | addItem | `/carrito/items` | `/api/carrito/items` | OK | OK | OK |
| cart.api.ts | updateItem | `/carrito/items/{itemId}` | `/api/carrito/items/{itemId}` | OK | OK | OK |
| cart.api.ts | removeItem | `/carrito/items/{itemId}` | `/api/carrito/items/{itemId}` | OK | OK | OK |
| cart.api.ts | clear | `/carrito` | `/api/carrito` | OK | OK | OK |
| checkout.api.ts | start | `/checkout` | `/api/checkout` | OK | OK | OK |
| checkout.api.ts | saveShipping | `/checkout/envio` | `/api/checkout/envio` | fixed `direccionEnvioId`/`direccionFacturaId` | OK | OK |
| checkout.api.ts | pay | `/checkout/pago` | `/api/checkout/pago` | OK | OK | OK |
| checkout.api.ts | getConfirmation | `/checkout/confirmacion/{orderNumber}` | `/api/checkout/confirmacion/{orderNumber}` | OK | fixed TS DTO | OK |
| orders.api.ts | getOrders | `/pedidos/mis-pedidos` | `/api/pedidos/mis-pedidos` | OK | OK | OK |
| orders.api.ts | getById | `/pedidos/{id}` | `/api/pedidos/{id}` | OK | fixed TS DTO | OK |
| orders.api.ts | cancelOrder | `/pedidos/{id}/cancelar` | `/api/pedidos/{id}/cancelar` | OK | OK | OK |
| orders.api.ts | getInvoiceByOrderId | `/facturas/pedido/{pedidoId}` | `/api/facturas/pedido/{pedidoId}` | OK | fixed typed response | OK |
| orders.api.ts | create | `/pedidos` | `/api/pedidos` | OK | OK | OK |
| profile.api.ts | getProfile | `/usuarios/perfil` | `/api/usuarios/perfil` | OK | OK | OK |
| profile.api.ts | updateProfile | `/usuarios/perfil` | `/api/usuarios/perfil` | fixed TS request | OK | OK |
| profile.api.ts | address CRUD | `/usuarios/direcciones/**` | `/api/usuarios/direcciones/**` | OK | OK | OK |
| wishlist.api.ts | getWishlist | `/interaccion/wishlist` | `/api/interaccion/wishlist` | OK | OK | OK |
| wishlist.api.ts | toggleWishlist | `/interacciones/productos/{id}/wishlist` | `/api/interacciones/productos/{id}/wishlist` | OK | OK | OK |
| seller analytics | getSellerSummary | `/analitica/vendedor/resumen` | `/api/analitica/vendedor/resumen` | OK | fixed typed response | OK |
| seller analytics | getAdminSummary | `/analitica/admin/resumen` | `/api/analitica/admin/resumen` | OK | fixed typed response | OK |
| seller-onboarding.api.ts | activate | `/usuarios/convertir-vendedor` | `/api/usuarios/convertir-vendedor` | fixed `ConvertirVendedorRequest` | OK | OK |
| admin.api.ts | analytics | `/analitica/admin/**` | `/api/analitica/admin/**` | OK | OK | OK |
| admin.api.ts | crearCategoria | `/catalogo/categorias` | `/api/catalogo/categorias` | OK | OK | OK |
| admin.api.ts | users | `/admin/usuarios/**` | `/api/admin/usuarios/**` | implemented backend + wired FE | OK | OK |
| admin.api.ts | products | `/admin/productos/**` | `/api/admin/productos/**` | implemented backend + wired FE | OK | OK |
| admin.api.ts | orders | `/admin/pedidos/**` | `/api/admin/pedidos/**` | implemented backend + wired FE | OK | OK |
| admin.api.ts | sellers | `/admin/vendedores/**` | `/api/admin/vendedores/**` | implemented backend + wired FE | OK | OK |
| admin.api.ts | config | `/admin/configuracion` | `/api/admin/configuracion` | implemented backend + wired FE | OK | OK |

## Track A - Step 3 Runtime Issues

| Flow | Issue | Status |
|---|---|---|
| Auth cycle | Session restore ran asynchronously and guards could execute before `me()` finished. | fixed |
| Auth cycle | Admin logout cleared local state without backend logout. | fixed |
| Catalog -> detail | Category URL could load before category id was known. | fixed |
| Catalog -> detail | Brand/rating filters and `masVendidos` sort were unsupported backend params. | fixed |
| Product detail | UI read removed/backend-missing fields and related products endpoint was missing. | fixed with DTO cleanup + fallback |
| Cart -> Checkout | Wishlist/rebuy add-to-cart did not hydrate cart badge immediately. | fixed |
| Cart -> Checkout | Quantity optimistic update did not reliably clear pending/revert on error. | fixed |
| Cart -> Checkout | Shipping step did not persist selected address id in backend payload. | fixed |
| Checkout confirmation | UI expected comprobante/factura URLs not returned by backend. | fixed |
| Profile | Perfil request typing lacked real optional backend fields. | fixed |
| Profile | Address fields verified as `tipo`, `esPrincipal`, `departamento`. | OK |
| Seller activation | Form sends required `ConvertirVendedorRequest`, refreshes `/usuarios/me`, updates auth state. | OK |
| Seller dashboard | Seller analytics endpoints match; product creation uses catalog product endpoint. | OK |
| Admin dashboard | Analytics paths and field names match Java DTO; period values are `DIARIO`/`SEMANAL`/`MENSUAL`. | OK |

## Track A - Step 4 Fixes

| Flow | Issue | File fixed | Fix applied |
|---|---|---|---|
| Auth | Guard race on refresh | `frontend/src/app/core/auth/auth-session.service.ts`, `frontend/src/app/app.config.ts` | Added app initializer awaiting `AuthApi.me()`. |
| Auth | Logout skipped backend | `frontend/src/app/features/admin/admin-shell/admin-shell.component.ts` | Calls `AuthApi.logout()` before local cleanup/navigation. |
| Catalog | Unsupported params and stale category id | `catalog.api.ts`, `catalog.component.ts/html`, `home.component.ts` | Real backend params only; reload after category resolution. |
| Product detail | Missing fields/endpoints | `api.models.ts`, `product-detail.component.ts/html`, `product-card.component.ts` | Removed fields not in Java DTOs; fallback products by category. |
| Cart | Optimistic quantity bug | `cart.component.ts` | Clears pending state and reverts on error. |
| Cart badge | Add-to-cart outside cart did not update shared state | `wishlist.component.ts`, `order-detail.component.ts` | Hydrates `CartUiService` from API response. |
| Checkout | Shipping payload missed address ids | `checkout-shipping.component.ts`, `api.models.ts` | Sends `direccionEnvioId` and `direccionFacturaId`. |
| Checkout | Confirmation expected unavailable URLs | `checkout-confirmation.component.ts/html`, `api.models.ts` | Removed unsupported download URL path. |
| Orders | Order detail used fields not in backend DTO | `order-detail.component.ts/html`, `api.models.ts` | Removed `numeroPedido`, `direccionEnvio`, `metodoPago`, URL fields. |
| Admin | Missing admin API endpoints | `admin.api.ts`, backend `admin/**` | Replaced `throwError` with real `/api/admin/**` calls. |

## Track A - Step 5 SSR

Checked patterns in component files:

- `localStorage.`
- `sessionStorage.`
- `window.`
- `document.getElementById`
- `document.querySelector`
- `document.body`

Result: no unguarded component occurrences found. Existing hits are guarded with `isPlatformBrowser`.

## Verification

- Frontend: `npx tsc -p tsconfig.app.json --noEmit` passed.
- Backend: `.\mvnw.cmd test` fails before Maven starts due wrapper script error; direct cached Maven starts but needs Maven Central and sandbox blocks network (`Permission denied: getsockopt`). No backend test result was produced in this environment.

## Track B - Implemented Locally

| Group | Endpoints | Files |
|---|---|---|
| B2 Users | `GET /api/admin/usuarios`, `GET /api/admin/usuarios/{id}`, `PATCH /api/admin/usuarios/{id}/rol`, `PATCH /api/admin/usuarios/{id}/estado`, `DELETE /api/admin/usuarios/{id}` | `backend/src/main/java/com/neogamin/proyecto_formativo/admin/api/ControladorAdminUsuarios.java`, `admin/aplicacion/ServicioAdminUsuarios.java`, `admin/api/dto/*Usuario*`, repo additions |
| B3 Products | `GET /api/admin/productos`, `PATCH /api/admin/productos/{id}/aprobar`, `PATCH /api/admin/productos/{id}/rechazar` | `ControladorAdminProductos.java`, `ServicioAdminProductos.java`, `RechazarProductoRequest.java` |
| B4 Orders | `GET /api/admin/pedidos`, `GET /api/admin/pedidos/{id}`, `PATCH /api/admin/pedidos/{id}/estado` | `ControladorAdminPedidos.java`, `ServicioAdminPedidos.java`, `CambiarEstadoPedidoRequest.java`, pedido repo additions |
| B5 Sellers | `GET /api/admin/vendedores`, `PATCH /api/admin/vendedores/{id}/aprobar`, `PATCH /api/admin/vendedores/{id}/suspender` | `ControladorAdminVendedores.java`, `ServicioAdminVendedores.java`, `VendedorAdminResponse.java`, repo additions |
| B6 Config | `GET /api/admin/configuracion`, `PUT /api/admin/configuracion` | `ControladorAdminConfiguracion.java`, `ServicioAdminConfiguracion.java`, `ConfiguracionPlataforma.java`, `ConfiguracionPlataformaRepositorio.java`, `V14__create_configuracion_plataforma.sql` |
| B7 Security | `/api/admin/**` restricted by method to `hasRole("ADMIN")` before catch-all auth rule | `ConfiguracionSeguridad.java` |
| B8 Frontend | Replaced admin `throwError` stubs with real calls and replaced admin placeholders with real list/form views | `frontend/src/app/features/admin/data-access/admin.api.ts`, `admin/users`, `admin/products`, `admin/orders`, `admin/sellers`, `admin/config` |

Notes:

- `Producto.estado` already exists as `EstadoGenerico` (`ACTIVO`/`INACTIVO`), so product rejection sets `INACTIVO` instead of adding a new product-state enum.
- SMTP is configured, but no product rejection email event/service was added; rejection only updates state.
- Railway deployment was not performed from this environment, so endpoints are implemented locally but not confirmed live on Railway.
