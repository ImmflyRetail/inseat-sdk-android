# Inseat Android SDK Documentation

This document provides comprehensive documentation for the Inseat Android SDK.

## Table of Contents

- [Initialization Methods](#initialization-methods)
- [Synchronization Methods](#synchronization-methods)
- [Data Management Methods](#data-management-methods)
- [Shop Information Methods](#shop-information-methods)
- [Product Management Methods](#product-management-methods)
- [Order Management Methods](#order-management-methods)
- [Permission Methods](#permission-methods)
- [Error Types](#error-types)
- [Data Models](#data-models)
- [How to setup sdk](#how-to-setup-sdk)

---

## SDK Methods Summary

| Method Name         | Description                                                             |
| ------------------- | ----------------------------------------------------------------------- |
| `initialize`        | Initializes the Inseat SDK with configuration parameters                |
| `start`             | Starts syncing data with POS devices via BLE mesh network               |
| `stop`              | Stops syncing data with POS devices and shuts down the SDK service      |
| `syncProductData`   | Downloads and caches product data from the server API                   |
| `setUserData`       | Sets the selected menu for filtering products                           |
| `observeShop`       | Subscribes to shop status updates and receives real-time notifications  |
| `fetchShop`         | Fetches shop data once (one-time operation)                             |
| `fetchMenus`        | Fetches available shop menus once                                       |
| `fetchSelectedMenu` | Fetches the currently selected menu                                     |
| `fetchCategories`   | Fetches available shop categories once                                  |
| `observeProducts`   | Subscribes to product updates and receives real-time notifications      |
| `fetchProducts`     | Fetches available shop products once                                    |
| `createOrder`       | Creates an order and sends it to the POS application                    |
| `observeOrders`     | Subscribes to order status updates and receives real-time notifications |
| `cancelOrder`       | Cancels an existing order and sends information to the POS application  |
| `fetchPromotions`  | Fetches all available promotions for the current shop                   |
| `applyPromotions`  | Calculates applied promotions based on list of cart items and selected currency  |
| `applyPromotion`   | Tries to apply one concrete promotion based on a list of cart items and selected currency. This method is supposed to be used when constructing promotion from Promotion Builder UI flow.    |
| `checkPermissions`  | Checks if the SDK has necessary permissions to operate                  |
| `getPermissionManager`| Provide PermissionManager instance. This manager makes request permission more flexible |

## Initialization Methods

### `initialize`

**Description:** Initializes the Inseat SDK with the provided configuration parameters. This method should be called when the app is launched.

**Signature:**

```kotlin
suspend fun initialize(applicationContext: Context, configuration: Configuration)
```

**Parameters:**

-`applicationContext` - Application context, `configuration` - Configuration object containing `apiKey, supportedICAOs, environment, shopAutoCloseTimeoutInSeconds` and `orderConfirmationTimeoutInSeconds`

**Return Type:** `Void` (throws on error)

**Errors:** `SDKInitializationException`

---

## Synchronization Methods

### `start`

**Description:** Starts syncing data with POS devices via BLE mesh network.

**Signature:**

```kotlin
fun start()
```

**Return Type:** `Void` (throws on error)

**Errors:** `SynchronizationException`

### `stop`

**Description:** Stops syncing data with POS devices and shuts down the SDK service.

**Signature:**

```kotlin
fun stop(): Result<Unit>
```

**Return Type:** `Result<Unit>`

**Errors:** None

---

## Data Management Methods

### `syncProductData`

**Description:** Downloads and caches product data from the server API.

**Signature:**

```kotlin
suspend fun syncProductData()
```

**Return Type:** `Void` (throws on error)

**Errors:** `NetworkException`

### `setUserData`

**Description:** Sets the selected menu for filtering products.

**Signature:**

```kotlin
suspend fun setUserData(userData: UserData)
```

**Parameters:**

- `userData` - UserData object containing menu selection

**Return Type:** `Void`

**Errors:** None

---

## Shop Information Methods

### `observeShop`

**Description:** Subscribes to shop status updates and receives real-time notifications.

**Signature:**

```kotlin
suspend fun observeShop(): StateFlow<ShopInfo>
```

**Return Type:** `StateFlow<ShopInfo>` - StateFlow that emits shop information updates

**Errors:** None (suspend function)

### `fetchShop`

**Description:** Fetches shop data once (one-time operation).

**Signature:**

```kotlin
suspend fun fetchShop(): ShopInfo
```

**Return Type:** `ShopInfo` - Shop information object

**Errors:** None (suspend function)

### `fetchMenus`

**Description:** Fetches available shop menus once.

**Signature:**

```kotlin
suspend fun fetchMenus(): List<Menu>
```

**Return Type:** `List<Menu>` - List of Menu objects

**Errors:** None (suspend function)

### `fetchSelectedMenu`

**Description:** Fetches the currently selected menu.

**Signature:**

```kotlin
fun fetchSelectedMenu(): Menu?
```

**Return Type:** `Menu?` - Selected Menu object or null if nothing is selected

**Errors:** None

### `fetchCategories`

**Description:** Fetches available shop categories once.

**Signature:**

```kotlin
suspend fun fetchCategories(): List<Category>
```

**Return Type:** `List<Category>` - List of Category objects

**Errors:** None (suspend function)

---

## Product Management Methods

### `observeProducts`

**Description:** Subscribes to product updates and receives real-time notifications.

**Signature:**

```kotlin
suspend fun observeProducts(category: Category? = null): StateFlow<List<Product>>
```

**Parameters:** `category` - optional argument to filter products by selected category and its subcategories

**Return Type:** `StateFlow<List<Product>>` - StateFlow that emits product list updates

**Errors:** None (suspend function)

### `fetchProducts`

**Description:** Fetches available shop products once.

**Signature:**

```kotlin
suspend fun fetchProducts(queryIds: List<Int> = emptyList(), category: Category? = null): List<Product>
```

**Parameters:**

`queryIds` - Optional list of product IDs to fetch (if empty, all products are fetched), `category` - optional argument to filter products by selected category and its subcategories

**Return Type:** `List<Product>` - List of Product objects

**Errors:** None (suspend function)

---

## Order Management Methods

### `createOrder`

**Description:** Creates an order and sends it to the POS application.

**Signature:**

```kotlin
suspend fun createOrder(order: Order, callback: (Result<Unit>) -> Unit)
```

**Parameters:**

- `order` - Order object containing all order information
- `callback` - Callback function for operation result

**Return Type:** `Void` (result via callback)

**Errors:** Result provided via callback

### `observeOrders`

**Description:** Subscribes to order status updates and receives real-time notifications.

**Signature:**

```kotlin
suspend fun observeOrders(): StateFlow<List<Order>>
```

**Return Type:** `StateFlow<List<Order>>` - StateFlow that emits order list updates

**Errors:** None (suspend function)

### `cancelOrder`

**Description:** Cancels an existing order and sends this information to the POS application.

**Signature:**

```kotlin
suspend fun cancelOrder(orderId: String, callback: (Result<Unit>) -> Unit)
```

**Parameters:**

- `orderId` - Order ID string
- `callback` - Callback function for operation result

**Return Type:** `Void` (result via callback)

**Errors:** Result provided via callback

---

## Promotion Management Methods

### `fetchPromotions`

**Description:** Fetches all available promotions for the current shop.

**Signature:**

```kotlin
suspend fun fetchPromotions(): List<Promotion>
```

**Parameters:**

- **Android:** None

**Return Type:**

- **Android:** `List<Promotion>` - Array of Promotion objects

**Errors:**

- **Android:** None

### `applyPromotions`

**Description:** Calculates applied promotions based on list of cart items and selected currency.

**Signature:**

```kotlin
suspend fun applyPromotions(cartItems: List<CartItem>, currency: String): PromotionResult
```

**Parameters:**

- **Android:** `cartItems` - list of current cart/basket items, `currency` - currently selected shop currency

**Return Type:**

- **Android:** `PromotionResult` - Result object with array of applied promotions

**Errors:**

- **Android:** None

### `applyPromotion`

**Description:** Tries to apply one concrete promotion based on a list of cart items and selected currency. This method is supposed to be used when constructing promotion from Promotion Builder UI flow.

**Signature:**

```kotlin
suspend fun applyPromotions(cartItems: List<CartItem>, currency: String): PromotionResult
```

**Parameters:**

- **Android:** `promotion` - promotion object which we try to apply to the cart/basket, `cartItems` - list of current cart/basket items, `currency` - currently selected shop currency

**Return Type:**

- **Android:** `PromotionResult` - Result object with array of applied promotions (no other promotion except that one from input parameters can be returned here).

**Errors:**

- **Android:** None

---
## Permission Methods

### `checkPermissions`

**Description:** Checks if the SDK has the necessary permissions to operate (Bluetooth, Local Network).

**Signature:**

```kotlin
fun checkPermissions(activity: ComponentActivity, doOnSuccess: () -> Unit)
```

**Parameters:**

- `activity` - Activity context
- `doOnSuccess` - Callback executed if permissions are granted

**Return Type:** `Void` (throws on error, executes callback on success)

**Errors:** `PermissionDeniedException`

### `getPermissionManager` (Android Only)

**Description:** his method returns the PermissionManager instance to manage permissions (request it). It is useful if you want to split requesting permissions to two steps: create the manager and request permissions later.

**Android Signature:**

```kotlin
fun getPermissionManager(activity: ComponentActivity): PermissionManager
```

**Parameters:**

- **Android:** `activity` - Activity context

**Return Type:**

- **Android:** `PermissionManager` - an interface that defines a method to check permissions.


**Errors:**

- **Android:** `PermissionDeniedException`
---

## Error Types

### `SDKInitializationException`

**Message:** "SDK is not initialized. Did you forget to call initialize()?"
**Description:** Thrown when SDK methods are called before initialization

### `PermissionDeniedException`

**Message:** "Permission not granted"
**Description:** Thrown when required permissions are not granted

### `SynchronizationException`

**Message:** "Mesh sync error"
**Description:** Thrown when BLE mesh synchronization fails

### `NetworkException`

**Message:** Custom message based on specific network error
**Description:** Thrown when network operations fail

### `StoreException`

**Message:** Custom message based on specific store error
**Description:** Thrown when local data store operations fail

### `SerializationException`

**Message:** Custom message based on specific serialization error
**Description:** Thrown when data serialization/deserialization fails

---

## Data Models

### `Menu`

Represents a menu item.

- **key**: `String` — Unique identifier for the menu item.
- **displayName**: `List<DisplayName>` — List of display names for the menu item in different locales.
- **companyId**: `Int` — Company identifier.
- **icao**: `String` — ICAO code.

### `DisplayName`

- **locale**: `String` — Locale code (e.g., "en", "es").
- **text**: `String` — Display name text in the specified locale.

### `Order`

Represents an order.

- **id**: `String` — Unique identifier for the order.
- **shiftId**: `String` — Shift identifier.
- **totalPrice**: `BigDecimal` — Total price of the order.
- **currency**: `String` — Currency code (e.g., "EUR").
- **items**: `List<OrderItem>` — List of order items.
- **status**: `OrderStatusEnum` — Status of the order.
- **customerSeatNumber**: `String` — Customer seat number.
- **createdAt**: `Date` — Creation date.
- **updatedAt**: `Date` — Last update date.

### `OrderStatusEnum`

- `PLACED`, `RECEIVED`, `PREPARING`, `CANCELLED_BY_PASSENGER`, `CANCELLED_BY_CREW`, `CANCELLED_BY_TIMEOUT`, `COMPLETED`

### `OrderItem`

- **itemId**: `Int` — Ordered item identifier.
- **name**: `String` — Item name.
- **quantity**: `Int` — Quantity ordered.
- **price**: `BigDecimal` — Price per unit.

### `Product`

Represents a product.

- **itemId**: `Int` — Unique product identifier.
- **itemMasterId**: `Int` — Product master identifier.
- **categoryId**: `Int` — Category identifier.
- **name**: `String` — Product name.
- **itemName**: `String` — Alternate name.
- **description**: `String` — Description.
- **base64Image**: `String` — Image in Base64.
- **itemType**: `Int` — Product type.
- **prices**: `List<ProductPrice>` — List of prices in different currencies.
- **startDate**: `Date` — Availability start date.
- **endDate**: `Date` — Availability end date.
- **quantity**: `Long` — Available quantity.

### `ProductPrice`

- **currency**: `String` — Currency code.
- **price**: `BigDecimal` — Price amount.

### `Category`

Represents a category.

- **id**: `Int` — Unique category identifier in storage.
- **categoryId**: `Int` — Category identifier.
- **name**: `String` — Category name.
- **sortOrder**: `Int` — Sort order.
- **subcategories**: `List<Subcategory>` — List of subcategories.

### `Subcategory`

- **id**: `Int` — Subcategory identifier.
- **name**: `String` — Subcategory name.
- **sortOrder**: `Int` — Sort order.

### `Shop`

Represents a shop.

- **id**: `String` — Shop identifier.
- **shiftId**: `String` — Shift identifier.
- **status**: `StatusEnum` — Shop status (`OPEN`, `ORDER`, `CLOSED`).
- **items**: `List<ShopItem>` — List of shop items.
- **crewLastSeen**: `Date` — Last crew seen timestamp.

### `ShopItem`

- **itemId**: `Int` — Item identifier.
- **prices**: `List<ShopItemPrice>` — List of prices.

### `ShopItemPrice`

- **currency**: `String` — Currency code.
- **price**: `BigDecimal` — Price amount.

### `UserData`

Represents user data in the SDK.

- **selectedMenu**: `Menu` — Currently selected menu for the user.

### `Configuration`

Represents SDK configuration.

- **apiKey**: `String` — API key for authentication.
- **orderConfirmationTimeoutInSeconds**: `Int` — Timeout for order operations (default: 120 seconds).

---

## How to setup sdk

1. Add to your root `settings.gradle.kts` file the following code:
    ```kotlin
    dependencyResolutionManagement {
      repositories {
         maven {
            url = uri("https://app-cdn.immflyretail.live/inseat-android-sdk/")
            credentials {
               username = "enter-your-username-here"
               password = "enter-your-password-here"
            }
         } 
      }
    }
    ```
2. Add to your module's `build.gradle.kts` file the following code:
   ```kotlin
   dependencies {
       implementation("com.immflyretail.inseat.sdk:inseat:0.1.11")
   }
   ```



Typically SDK usage:

1. Initialize the SDK
   ```kotlin
   InseatSdk.initialize(
       initialize(
                applicationContext = this@MainApplication,
                apiKey = "enter-your_api_key-here"
            )
   )
   ```
2. Call the `InseatSdk.getInstance().syncProductData()` method to sync the product data.
3. Call the `InseatSdk.getInstance().start` method to start the SDK.
4. Call the `InseatSdk.getInstance().fetchMenus` method to get all menus.
5. Call the `InseatSdk.getInstance().setUserData` method to choose menu.
6. Use methods like `observeShop`, `observeProducts` observe necessary data.
