# Frontend Context

## Framework and Version

- Framework: Angular `^21.2.0`
- Rendering: Angular browser app with optional Angular SSR and Express runtime
- Language: TypeScript `~5.9.2`
- Styling: Tailwind CSS `^4.1.12` plus global design tokens in `tailwind.config.js`
- Package manager: npm `10.9.4`

## Folder Structure

```text
frontend/
|-- angular.json
|-- package.json
|-- tailwind.config.js
|-- vercel.json
|-- docs/
|-- public/
|-- src/
|   |-- app/
|   |   |-- core/
|   |   |   |-- auth/
|   |   |   |-- http/
|   |   |   |-- interceptors/
|   |   |   |-- layout/
|   |   |   |-- models/
|   |   |-- features/
|   |   |   |-- admin/
|   |   |   |-- ai/
|   |   |   |-- auth/
|   |   |   |-- cart/
|   |   |   |-- catalog/
|   |   |   |-- checkout/
|   |   |   |-- home/
|   |   |   |-- orders/
|   |   |   |-- payments/
|   |   |   |-- product/
|   |   |   |-- profile/
|   |   |   |-- security/
|   |   |   |-- seller/
|   |   |   |-- wishlist/
|   |   |-- shared/
|   |   |   |-- chatbot-widget/
|   |   |   |-- pipes/
|   |   |   |-- ui/
|   |   |-- app.config.ts
|   |   |-- app.routes.ts
|   |   |-- app.ts
|   |-- environments/
|   |   |-- environment.ts
|   |   |-- environment.development.ts
|   |-- index.html
|   |-- main.ts
|   |-- main.server.ts
|   |-- server.ts
|   |-- styles.css
```

## Pages and Routes

Routes are defined in `src/app/app.routes.ts`.

| Route | Component or behavior | Guard | Purpose |
| --- | --- | --- | --- |
| `/admin` | `AdminShellComponent` | `authGuard`, `adminGuard` | Admin layout with nested admin navigation. |
| `/admin` | Redirects to `/admin/dashboard` | `authGuard`, `adminGuard` | Default admin landing route. |
| `/admin/dashboard` | `AdminDashboardComponent` | `authGuard`, `adminGuard` | Admin KPI and sales dashboard. |
| `/admin/usuarios` | `AdminUsersComponent` | `authGuard`, `adminGuard` | Admin user listing and user role/status actions. |
| `/admin/productos` | `AdminProductsComponent` | `authGuard`, `adminGuard` | Admin product moderation and category creation. |
| `/admin/pedidos` | `AdminOrdersComponent` | `authGuard`, `adminGuard` | Admin order listing and status updates. |
| `/admin/vendedores` | `AdminSellersComponent` | `authGuard`, `adminGuard` | Admin seller approval/suspension management. |
| `/admin/analitica` | `AdminAnalyticsComponent` | `authGuard`, `adminGuard` | Admin analytics charts and lists. |
| `/admin/configuracion` | `AdminConfigComponent` | `authGuard`, `adminGuard` | Platform configuration form. |
| `/` | `HomeComponent` inside `LayoutComponent` | Public | Storefront home page. |
| `/home` | `HomeComponent` | Public | Home alias. |
| `/catalog` | `CatalogComponent` | Public | Product catalog with search, filters, pagination, and cart actions. |
| `/catalogo` | `CatalogComponent` | Public | Spanish catalog alias. |
| `/ai/assistant` | `AiAssistantComponent` | Public | Local assistant/chat experience for shopping help. |
| `/ia/asistente` | `AiAssistantComponent` | Public | Spanish AI assistant alias. |
| `/ai/search` | `AiSearchComponent` | Public | AI-themed product search screen. |
| `/ia/busqueda` | `AiSearchComponent` | Public | Spanish AI search alias. |
| `/cart` | `CartComponent` | Public UI, auth-aware data | Cart page. |
| `/carrito` | `CartComponent` | Public UI, auth-aware data | Spanish cart alias. |
| `/checkout` | `CheckoutSummaryComponent` | `authGuard` | Checkout summary/start step. |
| `/checkout/shipping` | Redirects to `/checkout/envio` | `authGuard` after redirect | English shipping alias. |
| `/checkout/envio` | `CheckoutShippingComponent` | `authGuard` | Shipping address step. |
| `/checkout/payment` | Redirects to `/checkout/pago` | `authGuard` after redirect | English payment alias. |
| `/checkout/pago` | `CheckoutPaymentComponent` | `authGuard` | Payment step. |
| `/checkout/confirmation/:orderId` | Redirects to `/checkout/confirmacion/:orderId` | `authGuard` after redirect | English confirmation alias. |
| `/checkout/confirmacion` | `CheckoutConfirmationComponent` | `authGuard` | Confirmation page using last checkout state. |
| `/checkout/confirmacion/:orderId` | `CheckoutConfirmationComponent` | `authGuard` | Confirmation page by order number/id route param. |
| `/favoritos` | Redirects to `/perfil/favoritos` | `authGuard` after redirect | Favorites alias. |
| `/wishlist` | Redirects to `/perfil/favoritos` | `authGuard` after redirect | Wishlist alias. |
| `/perfil` | `ProfileComponent` | `authGuard` | Profile shell with nested account pages. |
| `/perfil` | `EditProfileComponent` child | `authGuard` | Default profile edit view. |
| `/perfil/editar` | `EditProfileComponent` | `authGuard` | Edit profile data. |
| `/perfil/direcciones` | `AddressesComponent` | `authGuard` | Manage saved addresses. |
| `/perfil/favoritos` | `ProfileWishlistComponent` | `authGuard` | Authenticated wishlist view. |
| `/perfil/seguridad` | `ProfileSecurityComponent` | `authGuard` | Profile security settings UI. |
| `/perfil/activar-vendedor` | `SellerOnboardingComponent` | `authGuard` | Seller activation form. |
| `/profile` | Redirects to `/perfil` | `authGuard` after redirect | English profile alias. |
| `/profile/edit` | Redirects to `/perfil/editar` | `authGuard` after redirect | English edit alias. |
| `/pedidos` | `OrdersComponent` | `authGuard` | Authenticated order history. |
| `/pedidos/:orderId` | `OrderDetailComponent` | `authGuard` | Order detail and invoice/cancel/rebuy actions. |
| `/orders` | Redirects to `/pedidos` | `authGuard` after redirect | English orders alias. |
| `/orders/:orderId` | Redirects to `/pedidos/:orderId` | `authGuard` after redirect | English order-detail alias. |
| `/seguridad` | Redirects to `/perfil/seguridad` | `authGuard` after redirect | Security alias. |
| `/metodos-pago` | `PaymentsComponent` | `authGuard` | Payment-method/payment status page. |
| `/seller/dashboard` | Redirects to `/vendedor/dashboard` | `authGuard`, `sellerGuard` after redirect | English seller dashboard alias. |
| `/vendedor/dashboard` | `SellerDashboardComponent` | `authGuard`, `sellerGuard` | Seller analytics/dashboard view. |
| `/vendedor/panel` | Redirects to `/vendedor/dashboard` | `authGuard`, `sellerGuard` after redirect | Spanish seller panel alias. |
| `/seller/store` | `SellerStoreComponent` | `authGuard`, `sellerGuard` | Seller store management view. |
| `/vendedor/tienda` | `SellerStoreComponent` | `authGuard`, `sellerGuard` | Spanish seller store alias. |
| `/product/:slug` | `ProductDetailComponent` | Public | Product detail by slug. |
| `/producto/:slug` | `ProductDetailComponent` | Public | Spanish product-detail alias. |
| `/catalogo/productos/:slug` | `ProductDetailComponent` | Public | Backend-like product route alias. |
| `/login` | `LoginComponent` | Public | Login page. Also used inside the layout auth modal. |
| `/register` | `RegisterComponent` | Public | Registration page. Also used inside the layout auth modal. |
| `/registro` | `RegisterComponent` | Public | Spanish registration alias. |
| `/forbidden` | `ForbiddenComponent` | Public | Access denied page. |
| `**` | Redirects to `/` | Public | Catch-all route. |

`features/security/pages/security.component.ts` exists but is not referenced by the router.

## Reusable Components and Utilities

| Component/utility | Purpose |
| --- | --- |
| `LayoutComponent` | Main public layout with header, footer, auth modal, chatbot widget, router outlet, and online/offline state. |
| `HeaderComponent` | Storefront navigation, search, category menu, profile menu, cart preview, logout, and auth modal triggers. |
| `FooterComponent` | Footer links and newsletter form placeholder. |
| `ChatbotWidgetComponent` | Floating compact assistant widget backed by the local `AiChatService`. |
| `CheckoutProgressComponent` | Step indicator for summary, shipping, payment, and confirmation checkout screens. |
| `NeoButtonComponent` | Design-system button with variants, sizes, loading state, and click output. |
| `NeoCardComponent` | Reusable card shell with padding and hover variants. |
| `NeoInputComponent` | ControlValueAccessor input/textarea with labels, hints, errors, icons, and password reveal. |
| `NeoSelectComponent` | ControlValueAccessor select with options, hints, and errors. |
| `ProductCardComponent` | Product tile with image, pricing, stock labels, wishlist toggle, cart event, and recently viewed tracking. |
| `NeoModalComponent` | Accessible modal with focus management, escape handling, body scroll lock, and size variants. |
| `NeoPaginationComponent` | Pagination control with page windowing and `pageChange` output. |
| `NeoBadgeComponent` | Status/category badge variants. |
| `NeoBreadcrumbComponent` | Router-linked breadcrumb trail. |
| `NeoSkeletonComponent` | Loading skeleton variants. |
| `NeoSpinnerComponent` | Loading spinner sizes. |
| `NeoToastComponent` / `NeoToastService` | Toast notifications using a `BehaviorSubject` stream and auto-dismiss timers. |
| `PageWrapperComponent` | Simple wrapper/layout projection component. |
| `AuthPromptComponent` | Reusable login prompt for cart, checkout, wishlist, orders, or generic contexts. |
| `CopPricePipe` | Formats numbers as Colombian pesos (`COP`). |

## State Management Approach

- Redux/NgRx/Zustand: _Not detected_
- Primary state approach: Angular signals and `computed()` values inside components and services.
- Async/API state: RxJS `Observable` streams returned by data-access services.
- Session state: `AuthStateService` stores normalized user data and JWT in `localStorage` under `neogaming.auth.user` and `neogaming.auth.token`.
- Cart UI state: `CartUiService` keeps cart items, totals, loaded flag, and errors in signals while syncing with the backend for authenticated users.
- Wishlist UI state: `WishlistUiService` keeps wishlist items and per-product interaction state in signals.
- Checkout state: `CheckoutStateService` stores shipping, payment method, selected address, draft, and last order in memory signals.
- Toast state: `NeoToastService` uses RxJS `BehaviorSubject`.
- Recently viewed products: `ProductCardComponent` stores items in `localStorage` under `neo_recent_products`.

## API Calls Made by the Frontend

All relative paths are resolved through `ApiClient` and the configured `apiBaseUrl`.

| Service | Method | Endpoint | Purpose |
| --- | --- | --- | --- |
| `AuthApi` | `POST` | `/auth/login` | Authenticate and receive JWT/user data. |
| `AuthApi` | `POST` | `/auth/registro` | Register a user. |
| `AuthApi` | `GET` | `/usuarios/me` | Restore/read current user session. |
| `AuthApi` | `POST` | `/auth/logout` | Revoke/logout current session. |
| `CatalogApi` | `GET` | `/catalogo/productos` | Product listing with filters/pagination. |
| `CatalogApi` | `GET` | `/catalogo/categorias/arbol` | Category tree. |
| `CatalogApi` | `GET` | `/catalogo/productos/buscar-natural` | Natural-language product search. |
| `CatalogApi` | `GET` | `/catalogo/productos/slug/{slug}` | Product detail by slug. |
| `CatalogApi` | `GET` | `/catalogo/productos/{productId}` | Product detail by id. |
| `CatalogApi` | _Not called_ | `/catalogo/productos/{idProducto}/relacionados` | TODO only; the service throws because no backend endpoint exists. |
| `CartApi` | `GET` | `/carrito` | Load authenticated cart. |
| `CartApi` | `POST` | `/carrito/items` | Add product to cart. |
| `CartApi` | `PATCH` | `/carrito/items/{itemId}` | Update item quantity. |
| `CartApi` | `DELETE` | `/carrito/items/{itemId}` | Remove cart item. |
| `CartApi` | `DELETE` | `/carrito` | Clear cart. |
| `CheckoutApi` | `POST` | `/checkout` | Start checkout and get summary. |
| `CheckoutApi` | `POST` | `/checkout/envio` | Save shipping/billing data. |
| `CheckoutApi` | `POST` | `/checkout/pago` | Process simulated payment. |
| `CheckoutApi` | `GET` | `/checkout/confirmacion/{orderNumber}` | Fetch order confirmation. |
| `OrdersApi` | `GET` | `/pedidos/mis-pedidos` | Paginated authenticated order list. |
| `OrdersApi` | `GET` | `/pedidos/{orderId}` | Order detail. |
| `OrdersApi` | `POST` | `/pedidos/{orderId}/cancelar` | Cancel an order. |
| `OrdersApi` | `GET` | `/facturas/pedido/{pedidoId}` | Invoice by order. |
| `OrdersApi` | `POST` | `/pedidos` | Create an order. |
| `ProfileApi` | `GET` | `/usuarios/perfil` | Current profile. |
| `ProfileApi` | `PUT` | `/usuarios/perfil` | Update profile. |
| `ProfileApi` | `GET` | `/usuarios/direcciones` | List addresses. |
| `ProfileApi` | `POST` | `/usuarios/direcciones` | Create address. |
| `ProfileApi` | `PUT` | `/usuarios/direcciones/{id}` | Update address. |
| `ProfileApi` | `DELETE` | `/usuarios/direcciones/{id}` | Delete address. |
| `ProfileApi` | `PATCH` | `/usuarios/direcciones/{id}/principal` | Mark address as default. |
| `SellerOnboardingApi` | `POST` | `/usuarios/convertir-vendedor` | Convert current user into seller. |
| `ProductApi` | `GET` | `/catalogo/productos/{productId}` | Product detail by id. |
| `ProductApi` | `GET` | `/catalogo/productos/slug/{slug}` | Product detail by slug. |
| `ProductApi` | `GET` | `/resenas/productos/{productId}` | Product reviews. |
| `ProductApi` | `POST` | `/resenas` | Create or update product review. |
| `ProductApi` | `DELETE` | `/resenas/{resenaId}` | Delete product review. |
| `WishlistApi` | `GET` | `/interaccion/wishlist` | Authenticated wishlist. |
| `WishlistApi` / `WishlistUiService` | `POST` | `/interacciones/productos/{productId}/wishlist` | Toggle wishlist state. |
| `WishlistUiService` | `POST` | `/interacciones/productos/{productId}/like` | Toggle like state. |
| `PaymentsApi` | `GET` | `/pagos/{pagoId}` | Payment detail. |
| `PaymentsApi` | `POST` | `/pagos/{pagoId}/aprobar` | Approve payment. |
| `AnalyticsApi` | `GET` | `/analitica/admin/resumen` | Admin analytics summary used by seller/admin data-access. |
| `AnalyticsApi` | `GET` | `/analitica/vendedor/resumen` | Seller analytics summary. |
| `AdminApi` | `GET` | `/analitica/admin/resumen` | Admin summary. |
| `AdminApi` | `GET` | `/analitica/admin/ventas-por-periodo` | Admin revenue over time. |
| `AdminApi` | `GET` | `/analitica/admin/top-vendedores` | Top sellers. |
| `AdminApi` | `GET` | `/analitica/admin/top-productos` | Top products. |
| `AdminApi` | `GET` | `/analitica/admin/ventas-por-categoria` | Sales by category. |
| `AdminApi` | `GET` | `/analitica/admin/metodos-pago` | Payment-method analytics. |
| `AdminApi` | `GET` | `/analitica/admin/pedidos-por-estado` | Order status analytics. |
| `AdminApi` | `POST` | `/catalogo/categorias` | Create category. |
| `AdminApi` | `GET` | `/admin/usuarios` | Admin user page. |
| `AdminApi` | `PATCH` | `/admin/usuarios/{id}/rol` | Change user role. |
| `AdminApi` | `PATCH` | `/admin/usuarios/{id}/estado` | Toggle user active state. |
| `AdminApi` | `DELETE` | `/admin/usuarios/{id}` | Delete/deactivate user. |
| `AdminApi` | `GET` | `/admin/productos` | Admin product page. |
| `AdminApi` | `PATCH` | `/admin/productos/{id}/aprobar` | Approve product. |
| `AdminApi` | `PATCH` | `/admin/productos/{id}/rechazar` | Reject product. |
| `AdminApi` | `GET` | `/admin/pedidos` | Admin order page. |
| `AdminApi` | `GET` | `/admin/pedidos/{id}` | Admin order detail. |
| `AdminApi` | `PATCH` | `/admin/pedidos/{id}/estado` | Update order state. |
| `AdminApi` | `GET` | `/admin/vendedores` | Admin seller page. |
| `AdminApi` | `PATCH` | `/admin/vendedores/{id}/aprobar` | Approve seller. |
| `AdminApi` | `PATCH` | `/admin/vendedores/{id}/suspender` | Suspend seller. |
| `AdminApi` | `GET` | `/admin/configuracion` | Read platform config. |
| `AdminApi` | `PUT` | `/admin/configuracion` | Update platform config. |

## External Libraries Used

| Library | Why it is used |
| --- | --- |
| `@angular/*` | Application framework, routing, forms, browser rendering, SSR, hydration, and HTTP client. |
| `rxjs` | Observable-based HTTP results, stream composition, and UI event handling. |
| `tailwindcss` / `@tailwindcss/postcss` | Utility-first styling and custom NeoGaming design tokens. |
| `lucide-angular` / `lucide` | Icon rendering in UI controls, nav, buttons, status displays, and pages. |
| `express` | Node/SSR server for Angular SSR output. |
| `tslib` | TypeScript runtime helpers. |
| `vitest` | Unit testing. |
| `jsdom` | DOM-like test environment. |
| `prettier` | Formatting tool. |

## Patterns and Conventions

- Angular standalone components are used throughout; no NgModules were detected.
- Feature modules are organized under `src/app/features/{domain}` with `pages`, `components`, and `data-access` folders where needed.
- Cross-cutting concerns live under `core`: auth state/session, guards, interceptors, API client, layout, and typed API models.
- Shared UI primitives live under `shared/ui` and are exported from `shared/ui/index.ts`.
- API access is centralized through `ApiClient`, which prepends `API_BASE_URL`.
- `authInterceptor` adds `Authorization: Bearer <token>` when a token exists.
- `errorInterceptor` centralizes handling for API errors.
- Auth, seller, and admin route protection use functional Angular guards.
- UI state favors Angular signals and `computed()` values instead of global stores.
- Forms use Angular reactive forms and reusable ControlValueAccessor inputs/selects.
- Styling relies on Tailwind utility classes plus a `neo` token namespace in `tailwind.config.js`.
- Several routes have English and Spanish aliases for the same page.
- `environment.development.ts` and `environment.ts` both point to `http://localhost:8080/api` and keep `useMockApi: false`.
- `mockApiInterceptor` exists for development/test support but is disabled by the current environment files.
