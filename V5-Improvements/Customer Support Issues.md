## **CRITICAL FIXES (Must Address Immediately)**

**Issue 3 - Basic calculation mistake**: The display shows 135+30+70 = 13600 instead of 235. This is a **core calculator functionality failure**  , have to reproduce the issue and check if its an arithmetic logic issue or state management issue

- **Decision**: Must fix immediately
- **Complexity**: Likely simple bug in arithmetic logic
- **Risk**: High - affects basic product credibility

**Issue 6 - Decimal quantity calculation errors**: Agriculture customers need precise decimal calculations (65.850 kg).

- **Decision**: Must fix immediately
- **Complexity**: Likely floating-point precision issue
- **Risk**: High - affects specific customer segment revenue

**Issue 2 & 8 - Printer compatibility issues**: Hardware integration problems.

- **Decision**: Requires more time (medium fix)
- **Complexity**: Driver/firmware compatibility
- **Risk**: Low-Medium - affects specific hardware combinations

## **MODERATE FIXES (Stable fixes needed)**

**Issue 4 - Wrong billing calculation with discounts**: ₹130 expected vs ₹120 actual when adding same SKU with discount.

- **Decision**: Can go in next stable release
- **Complexity**: Business logic error in billing module
- **Risk**: Medium - affects billing accuracy

**Issue 1 - Cannot re-add deleted products**: After deleting item in billing, same item can't be re-added.

- **Decision**: Can go in next stable release
- **Complexity**: Session/state management issue
- **Risk**: Medium - workflow disruption

**Issue 7 - Sales button not responsive**: After calculation→Sales→C→Sales sequence.

- **Decision**: Can go in next stable release
- **Complexity**: State management bug 
- **Risk**: Medium - user experience issue

## **INTEGRATION FIXES (Longer timeline)**

**Issue 5 - Store name sync issues**: Requires multiple sync operations to update.

- **Decision**: Requires more time (bigger fix)
- **Complexity**: Data synchronisation architecture
- **Risk**: Low - workaround exists


## **Recommended Priority Order**:

1. **Immediate**: Issues 3 & 6 , 2 and 8 (calculation errors and printer issues)
2. **Next Release**: Issues 1, 4, 7 (billing/workflow bugs)
3. **Future Release**: Issues  5(integration/sync issues)









4. Calculation error - 1. add 400 +  500 ,  press sales/expense button ( payment mode ) ( have to reproduce and fix ) , and then add other number  , error happens 
5. decimal error (1. 1.2 multiplied by 3.1 ) --- result - 3.72 ) ( don't press enter ) 2. press correct 2 times (  )  ( have to press correct 2 times  , pressing 1 time nothing happened)   3. have changed 3.1 to 3.3  and result was 39.6 ( incorrect )
6. mrc issue ( 1. make 500*5 --- m+ --- 600 * 6 --- m+ --- 700 * 7 ---   m+ ---- mrc  ) ----- incorrect value  ,,   2nd scenario ---  in the last after 700 * 7  ---- press mrc directly not m+  ----- result sometimes 6100 , and other time 0 
7. mrc callback ( feature need to add)
8.  set decimal precision ( feature to be added )
	1.  mu ( 200 + 100 + 50 ) -- enter -- markup ( mu ) ------  correct answer
	2.  ui change needed ( )
	3.  need to test with more edge cases
9. gt button ( grand total )
     1. ( feature to be added )

10. mrc button ( need to press mrc twice to clear , need to explore more from manual )
11. tax button ( + - ) -----  add 100 -- tax + --- tax --  result ( 21.24 ) -- answer should be 82 
12. function buttons should work properly ---- 
13.  - 3 + 5 = 2 ,     in our device ----  ( - ) is not getting put in the calculation , - operaor doesnt work on the first , basically if i start with operator instead of operand it doesnt get recognized





---

### **Issues to Reproduce and Fix / Features to Implement**

---

#### **1. Calculation Error (Sales/Expense Mode)**

- **Steps to Reproduce:**
    
    1. Add 400 + 500.
        
    2. Press the _Sales/Expense_ button (payment mode).
        
    3. Add another number.
        
- **Issue:** Calculation error occurs at this point.
    
- **Action:** Reproduce and fix.
    

---

#### **2. Decimal Calculation Bug**

- **Steps to Reproduce:**
    
    1. Multiply 1.2 by 3.1 (result shows 3.72, do **not** press Enter).
        
    2. Press the _Correct_ button twice (pressing once has no effect).
        
    3. Change 3.1 to 3.3.
        
- **Issue:** Final result shows as 39.6 (which is incorrect).
    
- **Action:** Investigate and fix.
    

---

#### **3. MRC Memory Calculation Issue**

- **Scenario 1:**
    
    1. 500 × 5 → M+
        
    2. 600 × 6 → M+
        
    3. 700 × 7 → M+
        
    4. Press _MRC_
        
    
    - **Issue:** Incorrect total value is displayed.
        
- **Scenario 2:**
    
    - After 700 × 7, press _MRC_ directly (without M+).
        
    - **Issue:** Result inconsistently shows either 6100 or 0.
        
- **Action:** Debug and fix memory handling.
    

---

#### **4. MRC Callback (New Feature)**

- **Action:** Implement MRC callback functionality.
    

---

#### **5. Set Decimal Precision (New Feature)**

- **Action:** Add support for setting decimal precision.
    

---

#### **6. MU (Markup) Functionality**

- **Test Case:**
    
    - Input: 200 + 100 + 50 → Enter → Press MU
        
    - **Result:** Correct answer.
        
- **Actions:**
    
    1. UI improvements needed.
        
    2. Test with more edge cases.
        

---

#### **7. GT (Grand Total) Button**

- **Action:** Feature to be added for _GT_ button functionality.
    

---

#### **8. MRC Button Behavior**

- **Issue:** Currently requires pressing _MRC_ twice to clear memory.
    
- **Action:** Explore expected behavior and align with manual.
    

---

#### **9. Tax Button (+/-) Calculation Error**

- **Steps to Reproduce:**
    
    - Input 100 → Tax + → Tax –
        
    - **Result:** Shows 21.24
        
    - **Expected:** 82
        
- **Action:** Correct the tax calculation logic.
    

---

#### **10. Function Buttons Consistency**

- **Action:** Ensure all function buttons work as expected across all modes.
    

---

#### **11. Operator at Start of Expression**

- **Issue:** Starting with a negative operator (e.g., -3 + 5) doesn't work.
    
- **Observation:** The minus sign is not recognized if used at the beginning of a calculation.
    
- **Action:** Fix parsing to accept operator as the first input when valid.
    

---

