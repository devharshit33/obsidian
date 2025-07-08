# Smart Calculator Application Reference

## Architecture Overview

The Smart Calculator application follows an MVC (Model-View-Controller) architecture with a state machine for navigation and flow control.

### 1. MVC Components

**Model Layer:**

- `calculator_tohands.h/c`: Core calculator data structures and operations
- `transaction_tohands.h/c`: Transaction persistence and retrieval
- `config_tohands.h/c`: Application configuration and shared data types

**View Layer:**

- `ui.h/c`: Base UI setup and resource declarations
- `ui_calculation.h/c`: Calculator screen implementation
- `ui_customer_details.h/c`: Customer information screen
- `ui_payment_qr.h/c`: QR code payment screen
- Various specialized UI components (billing, menu, etc.)

**Controller Layer:**

- `state_machine.h/c`: Main application state management
- `sm_calculation.h/c`: Calculator-specific state handling
- `calculator.h/c`: Calculator logic implementation
- Input handlers for keypad interactions

### 2. State Hierarchy

**Main States (`TOHANDS_MAIN_STATE`):**

- `TOHANDS_ONBOARDING`: Initial setup screens
- `TOHANDS_CALCULATION`: Main calculator mode
- `TOHANDS_BILLING`: Inventory and billing mode
- `TOHANDS_MENU`: Settings and system options
- `TOHANDS_CALC_SHUTDOWN`: Shutdown handling
- `TOHANDS_HISTORY`: Transaction history view

**Calculation States (`TOHANDS_CALCULATION_STATE`):**

- `TOHANDS_CALCULATION_NORMAL`: Standard calculator
- `TOHANDS_CALCULATION_PT`: Payment type selection
- `TOHANDS_CALCULATION_PAYMENT_QR`: QR payment display
- `TOHANDS_CALCULATION_SAVING`: Transaction saving process

**Calculator Operation States (`CALCULATION_NORMAL_STATE`):**

- `CALCULATION_NORMAL_NORMAL`: Regular input state
- `CALCULATION_NORMAL_CORRECT`: Correction state
- `CALCULATION_NORMAL_START_NEW`: New calculation after complete operation

## Key Data Structures

### 1. Calculator Expression

```c
typedef struct _expression {
    TAILQ_HEAD(expression_list, _opops) stack;
    double eval_value;
    double calc_gt;
    double calc_mrc;
    int is_last_operand_result;
    TAILQ_ENTRY(_expression) pointers;
} expression;
```

### 2. Transaction Data

```c
typedef struct {
    uint64_t id;
    TRANS_TYPE trans_type;
    PAYMENT_MODE payment_mode;
    ONLINE_PAYMENT_OPTION online_payment_option;
    double trans_amount;
    int64_t trans_time;
    char *trans_cust_name;
    char *trans_cust_num;
    char *trans_cust_remark;
    int64_t cashbook_user_id;
    char *operational_calculation;
    int sync_status;
} Trans_Data;
```

### 3. UI Update Structure

```c
typedef struct {
    Ui_Calculation_Cmd cmd;
    PAYMENT_MODE ui_payment_mode;
    ONLINE_PAYMENT_OPTION ui_online_payment_option;
    char charactor;
    char *param1;
    double param2;
    TRANS_TYPE param3;
    PAYMENT_MODE param4;
    ONLINE_PAYMENT_OPTION param5;
    const char *param6;
    const char *param7;
    const char *param8;
} Ui_Calculation_Data;
```

## Main Application Flows

### 1. Startup Flow

1. `tohands_ui_init()` initializes the UI
2. `state_machine_init()` sets up the state machine
3. Splash screen → Hello screen → Login check
4. If not logged in: Onboarding flow
5. If logged in: Direct to calculator

### 2. Calculator Flow

1. User inputs numbers through `calculator_push_operand()`
2. Operators are processed via `calculator_push_basic_arith_operator()`
3. Expression is evaluated with `calculator_evaluate()`
4. Result displayed in the total area

### 3. Transaction Flow

**Basic Transaction:**

1. Calculate amount → Press SALES/EXPENSE
2. Enter payment selection screen
3. Choose payment method (Cash/Card/Online/Later)
4. Optional: Enter customer details
5. Confirm and save transaction

**Online Payment:**

1. Select "ONLINE" payment mode
2. Choose payment app (PhonePe/Paytm/GPay/Others)
3. QR code display screen appears
4. User scans and completes payment
5. Transaction saved on confirmation

**Customer Details:**

1. Press USER button during payment selection
2. Enter mobile/customer ID
3. Enter customer name
4. Enter remarks
5. Return to payment selection

### 4. Transaction Saving

1. `transaction_tohands_save_to_database()` stores locally
2. Transaction marked as unsynced initially
3. Synchronization attempted when network available
4. Success screen shown briefly
5. Return to calculator mode

## Key Functions Reference

### State Machine Functions

- `state_machine_new_key_event()`: Entry point for all key events
- `state_machine_set_main_state()`: Changes the main application state
- `sm_calculation_get_state_current()`: Returns current calculation state

### Calculator Functions

- `calculator_push_operand()`: Adds numbers to expression
- `calculator_push_basic_arith_operator()`: Adds operators
- `calculator_evaluate()`: Processes the expression
- `calculator_clear_exp()`: Clears the current expression

### Transaction Functions

- `transaction_tohands_save_to_database()`: Stores transaction
- `transaction_tohands_get_unsynced_count()`: Retrieves pending sync count
- `transaction_tohands_update_synced()`: Updates sync status

### UI Functions

- `ui_calculation_screen_init()`: Sets up calculator screen
- `ui_customer_details_screen_init()`: Sets up customer details entry
- `ui_calculation_pt_footer_footer_update()`: Updates payment selection UI

## Event Handling Mechanism

1. Key press generates `CALC_KEY` event
2. `state_machine_new_key_event()` receives the event
3. Routes to appropriate state handler based on current state
4. Handler performs appropriate action
5. Creates UI update data structure
6. Sends UI message through LVGL messaging system
7. UI component event handler receives message
8. UI updates accordingly

## UI Components Breakdown

### 1. Main Calculator Screen

- Expression text area (`ui_calculation_operand_ta`)
- Total display area (`ui_total_ta`)
- Information panel with instructions
- Status indicators (MRC, GT)

### 2. Payment Selection Screen

- Payment amount display
- Transaction type indicator (Sales/Expense)
- Payment method buttons
- Instruction footer

### 3. Customer Details Screen

- Customer mobile/ID field
- Name field
- Remarks field
- Navigation buttons and instructions

## Payment Methods

### Available Payment Modes

- `PAYMENT_MODE_CASH`: Cash transactions
- `PAYMENT_MODE_ONLINE`: Digital payments
- `PAYMENT_MODE_CARD`: Card transactions
- `PAYMENT_MODE_LATER`: Credit/deferred payments

### Online Payment Options

- `PHONE_PE`: PhonePe app
- `PAYTM`: Paytm app
- `GPAY`: Google Pay
- `OTHERS`: Other UPI/payment apps

## Modifying the Application

### Adding New Screens

1. Create new UI file in `app/view/screens/`
2. Add screen initialization function
3. Add corresponding state in appropriate enum
4. Create state handler function in controller
5. Update state transitions

### Modifying Payment Flow

1. Add new payment mode to `PAYMENT_MODE` enum in `config_tohands.h`
2. Update `ui_calculation_pt_footer_footer_update()` to handle new mode
3. Add handler logic in `sm_calculation_pt_selection_new_key_event_handler()`
4. Update transaction saving to handle new mode

### Adding UI Components

1. Use LVGL to create new components
2. Follow existing patterns for event subscription
3. Create appropriate message handlers
4. Update UI based on state changes

### Maintaining UI Consistency

1. Use defined styles from `ui.h`
2. Follow existing UI layout patterns
3. Maintain similar visual language
4. Use the same message-based update mechanism

## Debugging Tips

1. Use `log_info()` for tracing state transitions
2. Monitor `accum` buffer for operand input issues
3. Check transaction storage with database tools
4. Verify UI updates by tracing message flow

---



- UI components register event handlers with `lv_obj_add_event_cb`
- Message IDs like `MSG_ID_CALCULATION` identify message types
- Data structures like `Ui_Calculation_Data` carry the event details

For example, when a user presses a number key:

1. `state_machine_new_key_event` is called
2. It dispatches to `state_machine_calculation_new_key_event_handler`
3. This calls `calculator_push_operand`
4. `calculator_push_operand` creates a `Ui_Calculation_Data` and sends a message
5. The UI handler `ui_calculation_normal_event_handler` processes the message
6. The UI is updated to show the pressed digit

Now that I have a good understanding of the code structure and flow, I can provide a comprehensive explanation of how it all works.

# Smart Calculator Application Analysis

This application is a comprehensive financial calculator with transaction recording, customer management, and receipt printing capabilities. It follows an MVC (Model-View-Controller) architecture and uses a state machine for navigation between different screens and modes.

## Architecture Overview

### 1. MVC Structure

- **Model**: Data structures and business logic
    - `calculator_tohands.h/c`: Defines calculator expressions, operators, and evaluation logic
    - `transaction_tohands.h/c`: Handles saving and retrieving transactions
    - `config_tohands.h/c`: Configuration settings and common data structures
- **View**: UI components using LVGL library
    - `ui.h/c`: Core UI setup and common components
    - `ui_calculation.h/c`: Calculator screen
    - `ui_customer_details.h/c`: Customer information input screen
    - Other specialized screens (payment, transaction confirmation, etc.)
- **Controller**: Application logic and state management
    - `state_machine.h/c`: Core state management
    - `sm_calculation.h/c`: Calculator-specific states
    - `calculator.h/c`: Calculator operations

### 2. State Management

The application uses a hierarchical state machine:

- **Main States** (`TOHANDS_MAIN_STATE`):
    - `TOHANDS_ONBOARDING`: Initial setup and login
    - `TOHANDS_CALCULATION`: Main calculator screen
    - `TOHANDS_BILLING`: Billing/inventory screen
    - `TOHANDS_MENU`: Settings menu
    - `TOHANDS_CALC_SHUTDOWN`: Shutdown screen
    - `TOHANDS_HISTORY`: Transaction history
- **Calculation States** (`TOHANDS_CALCULATION_STATE`):
    - `TOHANDS_CALCULATION_NORMAL`: Regular calculator mode
    - `TOHANDS_CALCULATION_PT`: Payment type selection
    - `TOHANDS_CALCULATION_PAYMENT_QR`: QR payment screen
    - `TOHANDS_CALCULATION_SAVING`: Transaction saving screen
- **Calculator Normal States** (`CALCULATION_NORMAL_STATE`):
    - `CALCULATION_NORMAL_NORMAL`: Regular input mode
    - `CALCULATION_NORMAL_CORRECT`: Correction mode
    - `CALCULATION_NORMAL_START_NEW`: Starting a new calculation

## Core Functionality and Flows

### 1. Calculator Engine

The calculator uses a queue-based structure to store expressions:

c

Copy

`typedef struct _expression {     TAILQ_HEAD(expression_list, _opops) stack;    double eval_value;    double calc_gt;  // Grand Total memory    double calc_mrc; // Memory Recall memory    int is_last_operand_result;    TAILQ_ENTRY(_expression) pointers; } expression;`

Operations are managed through these key functions:

- `calculator_push_operand`: Adds numbers/decimal point
- `calculator_push_basic_arith_operator`: Adds +, -, *, /
- `calculator_push_coplex_calc_operator`: Handles %, =, M+, M-
- `calculator_push_gst_operator`: Tax calculations
- `calculator_evaluate`: Evaluates the expression stack

### 2. Transaction Flow

The application supports multiple transaction flows:

1. **Basic Sales Flow**:
    - Calculate amount → Press SALES → Select payment method → Confirm → Save transaction
2. **Online Payment Flow**:
    - Calculate amount → Press SALES → Select "ONLINE" → Choose payment app (PhonePe/Paytm/GPay/Others) → Show QR code → Confirm → Save transaction
3. **Customer Details Flow**:
    - At payment type selection → Press USER → Enter customer number, name, remarks → Return to payment → Complete transaction

### 3. UI Event System

The UI uses LVGL's message system for updates:

- Event handlers subscribe to message IDs like `MSG_ID_CALCULATION`
- Functions send updates via structures like `Ui_Calculation_Data`
- UI components update based on received messages

Example message flow:

Copy

`Key Press → state_machine_new_key_event → calculator_push_operand →  Send UI message → ui_calculation_normal_event_handler → Update display`

## Detailed Component Analysis

### 1. Calculator Component

The calculator uses a stack-based approach:

- Each operation or operand is pushed onto a stack
- Operations are performed according to entry sequence
- Special operations like memory functions maintain separate values

Notable calculator features:

- Basic arithmetic (+, -, *, /)
- Percentage calculations
- Memory functions (MRC, M+, M-)
- Grand Total (GT) tracking
- Tax calculations with multiple rates (GST+, GST-)

### 2. Payment Processing

Payment modes include:

- `PAYMENT_MODE_CASH`: Cash transactions
- `PAYMENT_MODE_ONLINE`: Digital payments (apps)
- `PAYMENT_MODE_CARD`: Card payments
- `PAYMENT_MODE_LATER`: Credit/deferred payments

For online payments, supported apps:

- `PHONE_PE`
- `PAYTM`
- `GPAY`
- `OTHERS`

The UI changes based on payment selection, showing relevant buttons and guidance text.

### 3. Transaction Management

Transactions are stored with:

- Amount, type (credit/debit)
- Payment details
- Customer information
- Timestamp
- Sync status

The system handles both online and offline transactions, maintaining unsynchronized transactions for later upload.

## Key UI Screens and Components

1. **Main Calculator Screen**:
    - Expression display
    - Total display
    - Guidance footer
    - Status indicators (MRC, GT)
2. **Payment Type Selection**:
    - Transaction amount display
    - Payment method buttons
    - Payment app selection (for online)
    - Instruction footer
3. **Customer Details Screen**:
    - Mobile/ID input
    - Name input
    - Remarks input
    - Navigation buttons
4. **QR Payment Screen**:
    - Payment app logo
    - QR code display
    - Amount information
    - Confirmation instructions
5. **Transaction Confirmation**:
    - Success indicator
    - Transaction details
    - Receipt printing option

## Event Handling Mechanism

The application processes events through these steps:

1. **Key Capture**: Hardware keys generate `CALC_KEY` events
2. **State Routing**: `state_machine_new_key_event()` routes to appropriate handler
3. **State-Specific Processing**: Handlers like `state_machine_calculation_new_key_event_handler()`
4. **Action Execution**: Functions like `calculator_push_operand()` perform operations
5. **UI Update**: Messages sent through LVGL to update UI components

## UI Implementation Details

The UI uses several LVGL features:

- Text areas for input/output display
- Panels for organization
- Images for icons/logos
- Custom styles for visual design
- Message system for updates

Key UI update patterns:

c

Copy

`Ui_Calculation_Data *display_data = (Ui_Calculation_Data *)malloc(sizeof(Ui_Calculation_Data)); display_data->cmd = CALCULATION_ADD_CHAR; display_data->charactor = c; lock_ui_mutex(); lv_msg_send(MSG_ID_CALCULATION, display_data); unlock_ui_mutex();`

## Revamp Considerations

For UI revamp and flow changes, you should consider:

1. **State Machine Modifications**:
    - To add new screens, add new states to the appropriate enums
    - Implement corresponding handler functions in state machine files
2. **UI Component Updates**:
    - Modify existing screens in their respective .c files
    - Create new screen files for completely new interfaces
    - Update `ui.h` with any new image or font resources
3. **Flow Modifications**:
    - Update state transitions in the relevant state machine handlers
    - Add new event handling logic for new interactions
    - Ensure proper cleanup and initialization between screens
4. **Payment Flow Enhancements**:
    - To add payment methods, expand the `PAYMENT_MODE` enum
    - Add corresponding UI options in `ui_calculation_pt_footer_footer_update()`
    - Implement transaction handling for new payment types
5. **Backward Compatibility**:
    - Maintain existing function signatures where possible
    - Use the established event messaging pattern for UI updates
    - Follow the component initialization pattern when adding screens
