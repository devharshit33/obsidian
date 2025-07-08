### Data Structure & Behaviour Guidelines

#### Static Identifiers

- **TID**: Always remains the same.
    
- **DSN**: Represents the device's serial number.
    
- **Device ID**: Composed of `DSN + Random UUID`.
    
    - Generate the random UUID **once** initially (referred to as **UUID1**).
        

---

### Tracking & Data Flow

#### At App Launch (Before Login):

1. **Send**:
    
    - `Device ID = DSN + UUID1`
        
2. If **TID is not assigned**:
    
    - Send `Device ID = DSN + UUID1`
        
3. If **TID does not match API response**:
    
    - Generate **UUID2**
        
    - Send `Device ID = DSN + UUID2`
        

---

#### After User Logs In:

1. **Send**:
    
    - `User ID = Cashbook User ID`
    - Also send device id too 
        

---

#### On Logout:

1. **Send**:
    
    - `User ID = Cashbook User ID`
    - along with device id 
        
2. **After Successful Logout**:
    
    - Generate **UUID3**
        
    - Send `Device ID = DSN + UUID3`
        

---

1. **At App Start:**
    - If there's no UUID or it's invalid, a new one will be generated (UUID1)
    - The analytics will send device ID as DSN + UUID1
    - The analytics event will be sent in a separate thread without blocking the app
2. **When TID Changes:**
    - When `api_tohands_get_tid_by_serial_number` detects a TID change
    - A new UUID (UUID2) will be generated
    - Future analytics will use DSN + UUID2 for device ID
3. **After Login:**
    - Both user ID (cashbook_user_id) and device ID will be sent to Mixpanel
    - The device ID will be the current DSN + UUID
4. **After Logout:**
    - A new UUID (UUID3) will be generated
    - Future analytics will use DSN + UUID3 for device ID


---



TO BE REVIEWED
1. events more than 5 in 1 sec , skip that 



To ask - 
1. can we store the data locally in the db or in memory ? - NO
2. do i  need to use batching if internet is not there , what to do  - NO
3. retry how many time if api is giving error - 3 times for 30 seconds





Issues 
- [x] calculator_user_logged_out - logout event is not going to mixpanel
- [x] calculator_application_started - comes only first time  , after restarted the device but nothing came 
- [x] crash happening after logout
- [x] send $wifi instead of wifi 
- [x] also send $os
- [x] send only cashbook user id on logout , no device id 

















