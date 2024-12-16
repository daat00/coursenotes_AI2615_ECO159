Android 提供了多种存储 API 供开发者选择，适合不同类型的数据存储需求。以下是常见的存储 API 及其持久化位置：

---

## **1. SharedPreferences**
### **适用场景：**
- 存储少量键值对数据（如用户设置、配置信息等）。

### **持久化位置：**
- **文件路径**：`/data/data/<package_name>/shared_prefs/<preference_name>.xml`
- 数据以 XML 格式存储。

---

## **2. Internal Storage (内部存储)**
### **适用场景：**
- 存储私有文件（用户无法直接访问）。
- 文件如配置、缓存数据。

### **持久化位置：**
- **路径**：`/data/data/<package_name>/files`
- 可以通过以下代码获取路径：
  ```java
  Context context = getApplicationContext();
  File directory = context.getFilesDir();  // /data/data/<package_name>/files
  ```
- 文件只能被该应用访问，卸载应用时会被清除。

---

## **3. External Storage (外部存储)**
### **适用场景：**
- 存储需要与用户或其他应用共享的文件（如图片、视频、下载的文件）。
- 分为**私有目录**和**公共目录**。

### **持久化位置：**
1. **私有目录：**
   - **路径**：`/storage/emulated/0/Android/data/<package_name>/files/`
   - 示例代码：
     ```java
     File externalPrivateDir = context.getExternalFilesDir(null); // 应用私有外部目录
     ```
   - 卸载应用时会被删除。

2. **公共目录：**
   - **路径**：公共目录在 `/storage/emulated/0/` 下，通常包含 `Documents`, `Downloads`, `Pictures` 等。
   - 示例代码：
     ```java
     File publicDir = Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DOWNLOADS);
     ```
   - 数据不会随应用卸载而被删除。

---

## **4. SQLite Database**
### **适用场景：**
- 存储结构化数据（如记录、表单、用户数据等）。

### **持久化位置：**
- **文件路径**：`/data/data/<package_name>/databases/<database_name>`
- 示例代码：
  ```java
  SQLiteDatabase db = context.openOrCreateDatabase("my_database", MODE_PRIVATE, null);
  ```

---

## **5. Room Database (SQLite 的抽象层)**
### **适用场景：**
- 复杂的本地数据存储，ORM 支持。
- 提供强大的注解和数据访问层。

### **持久化位置：**
- 同 SQLite：`/data/data/<package_name>/databases/<database_name>`

---

## **6. Content Provider**
### **适用场景：**
- 在应用间共享数据（如联系人、媒体库数据）。
- 可以结合其他存储方式（如 SQLite 或文件）。

### **持久化位置：**
- 数据的存储方式取决于具体实现，例如可能是 SQLite、文件存储等。

---

## **7. Network Storage (如 Firebase Realtime Database, Firestore, 或 REST API)**
### **适用场景：**
- 数据存储在云端（如用户数据、应用设置）。
- 可用于多设备同步。

### **持久化位置：**
- 存储在远程服务器或云平台上，依赖网络访问。

---

## **8. Jetpack DataStore**
### **适用场景：**
- 用于取代 `SharedPreferences`，支持数据类型安全和异步操作。
- 提供两种模式：
  - **Preferences DataStore**（键值存储）。
  - **Proto DataStore**（基于 Protobuf 的结构化存储）。

### **持久化位置：**
- 数据存储为文件，路径类似：
  - `/data/data/<package_name>/files/datastore/<filename>.pb`

---

## **9. Cache Storage**
### **适用场景：**
- 存储临时数据（如网络请求缓存、图片缓存）。
- 不保证数据持久性，系统可能在存储空间不足时清除。

### **持久化位置：**
- **内部缓存路径**：`/data/data/<package_name>/cache/`
- **外部缓存路径**：`/storage/emulated/0/Android/data/<package_name>/cache/`

示例代码：
```java
File cacheDir = context.getCacheDir(); // 内部缓存路径
File externalCacheDir = context.getExternalCacheDir(); // 外部缓存路径
```

---

## **10. Android KeyStore**
### **适用场景：**
- 存储敏感数据（如加密密钥、认证信息）。
- 提供硬件安全模块支持。

### **持久化位置：**
- 密钥存储在安全的硬件模块或系统区域（无法直接查看）。

---

### **总结表格**

| 存储方式              | 持久化位置                                   | 卸载是否删除 | 适用场景                   |
|-----------------------|--------------------------------------------|--------------|---------------------------|
| SharedPreferences     | `/data/data/<package_name>/shared_prefs`   | 是           | 键值对配置数据            |
| Internal Storage      | `/data/data/<package_name>/files`          | 是           | 私有文件                  |
| External Storage 私有 | `/storage/emulated/0/Android/data/`         | 是           | 应用专属外部文件          |
| External Storage 公共 | `/storage/emulated/0/`                      | 否           | 用户/应用共享文件         |
| SQLite/Room Database  | `/data/data/<package_name>/databases`      | 是           | 结构化数据存储            |
| Cache Storage         | `/data/data/<package_name>/cache/`         | 是           | 临时数据                  |
| DataStore             | `/data/data/<package_name>/files/datastore`| 是           | 代替 SharedPreferences    |
| KeyStore              | 安全硬件或系统安全区域                      | -            | 存储加密密钥              |

如果你有具体存储场景或需要示例代码，可以继续询问！
