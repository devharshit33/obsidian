base screen

1. **Base Screen** (960x320 pixels):
    
    - Creates a main screen with a light gray background (`#F6F6F6`)
    - Makes it non-scrollable
    - Acts as the root container for other elements
2. **Main Container** (960x290 pixels):
    
    - Creates a slightly darker gray container (`#ECECEC`)
    - Positioned in the center of the base screen
    - Has a shadow effect around it
    - Non-scrollable
    - Can receive messages/events through a callback
3. **Two Status Icons**:
    
    - **MRC Status Icon**:
        
        - Initially hidden
        - Positioned at x:70, y:315
        - Can receive status updates through messages
    - **GT Status Icon**:
        
        - Initially hidden
        - Positioned at x:220, y:315
        - Can receive status updates through messages


![[Pasted image 20250311095149.png]]


Ui_status_bar_screen_init();
### Status Bar Overview

The function `ui_status_bar_screen_init` creates a status bar that appears to be positioned at the top of a screen with the following characteristics:

- Width: 960 pixels
- Height: 30 pixels
- Background color: #ECECEC (light gray)
- Has a shadow effect

### Status Bar Elements

The status bar contains several indicators:

1. **Unsynchronized Transactions Counter**
    
    - A button panel showing number of pending transactions
    - Shows a number (initialized as "0")
    - Uses custom font `ui_font_AVE_20`
2. **Sync Status**
    
    - An image indicator showing synchronization status
    - Uses `ui_img_sync_png` image
3. **Connectivity Indicators**
    
    - GSM status icon (initially hidden)
    - WiFi status icon (initially hidden)
    - Bluetooth status icon
    - Each has its own event callback for status updates
4. **Battery Information**
    
    - Battery status icon (initialized with `battery_4` image)
    - Battery percentage label
    - Uses custom font for percentage display


![[Pasted image 20250311100530.png]]### Explanation of the Visual Elements:

1. **Unsync Transactions Panel**:
    
    - Positioned on the left side of the status bar.
    - Displays the number of unsynchronized transactions (initially "0").
2. **Sync Status Icon**:
    
    - Positioned next to the Unsync Transactions Panel.
    - Displays the sync status using an image (`ui_img_sync_png`).
3. **GSM Icon**:
    
    - Positioned next to the Sync Status Icon.
    - Displays the GSM signal status using an image (`ui_img_gsm_png`).
4. **WiFi Icon**:
    
    - Positioned next to the GSM Icon.
    - Displays the WiFi signal status using an image (`ui_img_wifi_png`).
5. **Bluetooth Icon**:
    
    - Positioned next to the WiFi Icon.
    - Displays the Bluetooth status using an image (`ui_img_bluetooth_png`).
6. **Battery Icon**:
    
    - Positioned next to the Bluetooth Icon.
    - Displays the battery status using an image (`battery_4`).
7. **Battery Percentage Label**:
    
    - Positioned next to the Battery Icon.
    - Displays the battery percentage (initially empty).

### Notes:

- The icons for WiFi, GSM, and Bluetooth are initially hidden and will be shown based on the received events.
- The battery icon and percentage label will update based on the battery status events.
		- The status bar has a light gray background with a shadow effect.






Calculation Screen - ### Key Components and Flow:

1. **Header Inclusions and Definitions:**
    
    - The code includes various header files required for the UI, state machine, transaction handling, and other functionalities.
    - Several constants are defined for positioning symbols on the screen.
2. **Global Variables:**
    
    - Various `lv_obj_t` pointers are declared for different UI components like panels, footers, text areas, etc.
    - State variables are declared to keep track of the current state of the calculation process.
3. **Function Declarations:**
    
    - Functions are declared for initializing the footer, handling events, and updating the UI based on different states.
4. **Event Callback Function:**
    
    - `ui_calculation_panel_event_cb` is the main event callback function that handles events based on the current state of the calculation.
    - It retrieves the event target and message, processes the message payload, and calls appropriate event handlers based on the current state.
5. **Event Handlers:**
    
    - `ui_calculation_normal_event_handler`: Handles events in the normal calculation state, such as adding/deleting characters, updating the total, and showing icons.
    - `ui_calculation_pt_event_handler`: Handles events in the PT (Payment Type) state.
    - `ui_calculation_pt_selection_event_handler`: Handles events related to PT selection.
    - `ui_calculation_pt_cust_details_event_handler`: Handles events related to customer details in the PT state.
    - `ui_calculation_payment_qr_event_handler`: Handles events related to payment QR code.
    - `ui_calculation_saving_event_handler`: Placeholder for handling saving events.
6. **UI Initialization Functions:**
    
    - `ui_calculation_screen_init`: Initializes the main calculation screen, creates the calculation panel, and sets up the initial state.
    - `ui_calculation_footer_init_nologin`: Initializes the footer when the user is not logged in, displaying a message to log in.
    - `ui_calculation_footer_init`: Initializes the footer for logged-in users, displaying transaction information.
    - `ui_calculation_pt_footer_init`: Initializes the footer for the PT state, updating the UI based on the transaction type.
    - 1. - `ui_calculation_pt_footer_footer_update`: Updates the footer based on the selected payment mode.
7. **Helper Functions:**
    
    - `ui_calculation_del_operand`: Deletes characters from the operand text area until an operator is encountered.
    - `ui_calculation_customer_details_init`: Initializes the customer details screen.
    - `ui_calculation_sync_sucess_helper`: Displays a success message when a transaction is saved.

### Flow:

1. **Initialization:**
    
    - The `ui_calculation_screen_init` function is called to set up the main calculation screen and initialize the UI components.
    - Depending on the login status, either `ui_calculation_footer_init_nologin` or `ui_calculation_footer_init` is called to set up the footer.
2. **Event Handling:**
    
    - When an event occurs, `ui_calculation_panel_event_cb` is triggered.
    - The function processes the event and calls the appropriate event handler based on the current state (`ui_calculation_state_current`).
3. **State Transitions:**
    
    1. - The event handlers (`ui_calculation_normal_event_handler`, `ui_calculation_pt_event_handler`, etc.) handle specific commands and update the UI accordingly.
    - State transitions are managed by updating the state variables (`ui_calculation_state_current`, `ui_calculation_pt_state_current`, etc.).
4. **UI Updates:**
    
    - The UI is updated dynamically based on the events and state transitions, ensuring that the correct information is displayed to the user.





Calculation screen
## System Architecture

The system follows a classic MVC (Model-View-Controller) pattern:

1. **View Layer** (`ui_calculation.c`): Handles the UI elements and user interactions
2. **Controller Layer** (`sm_calculation.c`): Manages the application state and business logic
3. **Model Layer** (referenced from other files): Handles data and operations

## State Machine Design

The system uses a sophisticated state machine to manage different operational modes:

### Main States (`TOHANDS_CALCULATION_STATE`)

- `TOHANDS_CALCULATION_NORMAL`: Regular calculator mode
- `TOHANDS_CALCULATION_PT`: Payment Type selection mode
- `TOHANDS_CALCULATION_PAYMENT_QR`: QR code display for online payments
- `TOHANDS_CALCULATION_SAVING`: Transaction saving state

### Payment Type Sub-States (`CALCULATION_PT`)

- `CALCULATION_PT_SELECTION`: For selecting payment method
- `CALCULATION_PT_CUST_DETAILS`: For entering customer information

### Customer Details Sub-States (`CALCULATION_PT_CUST_DETAILS_ADD`)

- `CALCULATION_PT_CUST_DETAILS_TYPE_NUM`: For entering customer number
- `CALCULATION_PT_CUST_DETAILS_TYPE_NAME`: For entering customer name
- `CALCULATION_PT_CUST_DETAILS_TYPE_REMARKS`: For entering transaction remarks

## UI Components

The UI is built using LVGL (Light and Versatile Graphics Library):

1. **Main Calculation Panel** (`ui_calculation_panel`): Container for all elements
2. **Operand Text Area** (`ui_calculation_operand_ta`): Shows the current input
3. **Total Text Area** (`ui_total_ta`): Shows calculation result
4. **Footer** (`ui_calculation_footer`): Contains action buttons and info
5. **Payment Type Footer** (`ui_calculation_footer_footer`): Shows payment options
6. **Customer Details Panel**: For entering customer information

## Transaction Workflow

### 1. Calculator Mode

- User enters amounts and performs calculations
- System shows input in `ui_calculation_operand_ta` and results in `ui_total_ta`
- Calculator supports standard operations plus tax calculations
- Special keys: M+, M-, MRC, GT (Grand Total)

### 2. Transaction Initiation

When user presses SALES or EXPENSE:

- System switches to `TOHANDS_CALCULATION_PT` state
- Shows payment type options in the footer
- SALES creates a credit transaction, EXPENSE creates a debit transaction

### 3. Payment Type Selection

In `CALCULATION_PT_SELECTION` state:

- Default payment type is CASH
- Pressing SALES/EXPENSE cycles through payment types:
    - CASH → ONLINE → CARD → LATER (Pay later/due)
- If ONLINE is selected, pressing SHIFT cycles through apps:
    - PHONE_PE → PAYTM → GPAY → OTHERS
- Payment type shown in a button in the footer

### 4. Customer Details Entry (Optional)

When USER button is pressed:

- System enters `CALCULATION_PT_CUST_DETAILS` state
- Shows a form for entering:
    - Customer phone number
    - Customer name
    - Remarks
- Navigation using RIGHT/LEFT keys
- Multi-tap input for entering text (similar to old mobile phones)
- EQL key confirms and returns to payment selection

### 5. Payment Processing

When EQL is pressed in payment selection:

- For ONLINE payments:
    - Enters `TOHANDS_CALCULATION_PAYMENT_QR` state
    - Shows QR code for the customer to scan
    - EQL confirms payment received
- For other payment types:
    - Directly enters `TOHANDS_CALCULATION_SAVING` state

### 6. Transaction Saving

In saving state:

- Saves transaction to database (`transaction_tohands_save_to_database`)
- Signals for synchronization (`synchronization_signal_for_transaction`)
- Shows success message
- Returns to calculator mode after 2 seconds

## Payment Types

The system supports several payment methods:

1. **CASH**: Traditional cash payment
2. **ONLINE**: UPI-based payments (PhonePe, Paytm, Google Pay, Others)
3. **CARD**: Card-based payments
4. **LATER**: Credit transactions to be paid later

## Event Handling System

The system handles events in a hierarchical manner:

1. UI events are captured in `ui_calculation_panel_event_cb`
2. This function dispatches to different handlers based on current state:
    - `ui_calculation_normal_event_handler`
    - `ui_calculation_pt_event_handler`
    - `ui_calculation_payment_qr_event_handler`
    - `ui_calculation_saving_event_handler`
3. In the controller layer, key presses are handled by:
    - `state_machine_calculation_new_key_event_handler`: Main dispatcher
    - State-specific handlers like:
        - `state_machine_calculation_normal_new_key_event_handler`
        - `sm_calculation_pt_selection_new_key_event_handler`
        - `sm_calculation_pt_cust_details_type_num_new_key_event_handler`

## Communication Between Components

The UI and controller communicate through LVGL's messaging system:

- Controller sends messages with `lv_msg_send(MSG_ID_CALCULATION, display_data)`
- UI receives these in `ui_calculation_panel_event_cb`
- Data is passed via `Ui_Calculation_Data` structures

## Threading and Synchronization

The system employs thread safety mechanisms:

- Mutex locks during UI updates: `lock_ui_mutex()` and `unlock_ui_mutex()`
- Asynchronous sound playing: `sounds_tohands_play_sound()`
- Timer for automatic state transitions

## Key Functions

### UI Layer (ui_calculation.c)

- `ui_calculation_screen_init`: Creates and configures UI elements
- `ui_calculation_footer_init`: Sets up the footer based on login status
- `ui_calculation_pt_footer_init`: Creates payment type selection UI
- `ui_calculation_pt_footer_footer_update`: Updates UI based on payment type
- `ui_calculation_customer_details_init`: Creates customer details form
- `ui_calculation_sync_sucess_helper`: Shows transaction success screen

### Controller Layer (sm_calculation.c)

- `sm_calculation_init`: Sets up the state machine
- `state_machine_calculation_new_key_event_handler`: Main key event router
- `sm_calculation_pt_selection_new_key_event_handler`: Handles payment selection
- `sm_calculation_pt_cust_details_new_key_event_handler`: Handles customer details input
- `sm_calculation_payment_qr_new_key_event_handler`: Handles QR payment flow
- `sm_calculation_saving_new_key_event_handler`: Handles transaction saving

## Areas for UI Revamp