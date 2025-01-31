# Identified Issues and Implemented Solutions

## 1. Identified Issues

1. **groupCD was not working correctly**
   - All of the requests were being directed only to `CHE001` database, without including `HAV001` and `BAI001`.

2. **Route parameters (`VIN` and `groupCD`) were not handled properly**
   - Issues with validation of these parameters.

3. **Policy rule was not working as expected**
   - Still unresolved.

---

## 2. Implemented Solutions

### **Issue 1: groupCD Not Working Correctly**
- The issue occurred because the `groupCD` parameter was mistakenly referenced as `dataBasePath` in the route communicating with `tinydog2`.
- Now, `groupCD` must be correctly sent as `CHE001`, `HAV001`, or `BAI001`.

### **Issue 2: Handling of Route Parameters (`VIN` and `groupCD`)**
- The request now explicitly requires both `groupCD` and `VIN` parameters.
- Error messages have been improved to be more user-friendly and clear.

#### **Use Cases and Improved Error Messages:**

- **Invalid groupCD** → `GroupCD: ADADA is invalid`
- **Blank groupCD** → `The field groupCD is required`
- **VIN Not Found** → `The VIN: adda was not found`
- **Blank VIN** → `The field VIN is required`
- **Both VIN and groupCD blank** → `The fields VIN and groupCD are required`

### **Refactoring Configuration Loading**
- Previously, every request loaded rows from the `config.dbf` table on //tables.
- I refactored the code to avoid unnecessary reloading, improving performance.

---

## 3. Updates Performed

- Updated `tinydog2` and `IAL IDMS` repositories on GitHub.
- Executed CI/CD with `IAL-IDMS` to deploy the service on the development server.
- Manually updated `TinyDog2` on the `ial-dev` machine.

---

## 4. Testing and Documentation

I have prepared a `collection.json` file with documentation to facilitate testing.

(COLOCAR AQUI O LINK DO collection.json)

### **Testing Instructions:**
1. Before sending the `AA` request, log in using the `login` request.
2. The generated token is automatically applied to `AA` requests via a script, so you don’t need to copy and paste it manually.
3. After logging in, test the following URIs in Postman:

- **CHE001** → `vin=LVVDB21B0PD122695&groupCD=CHE001`
- **BAI001** → `vin=LNBMCUAK6PD887478&groupCD=BAI001`
- **HAV001** → `vin=LGWFF6A53PH917247&groupCD=HAV001`

### **Swagger Link in Development Environment:**
[Swagger Dev](https://ial-api-dev.iautologics.co.za/swagger/index.html)

---

## 5. Pending Issues

### **Fixing the Policy Rule**
There are still issues with the policy rule implementation, as identified in ClickUp.

The current logic is as follows:

**Do Not Assist** when:
- The war was canceled **or**
- (`inception` + `months` < `today`) **or**
- `inception` has not yet occurred.

**Assist** if none of the above conditions are met.

 I thought it would be the correct rule but i think that i was wrong.
 Could you explain me more about that?

---

#Assigned: Victor Almada

