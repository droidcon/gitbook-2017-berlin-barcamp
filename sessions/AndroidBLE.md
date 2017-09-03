# Open discussion about Android BLE & new Android O service limitations

### [Chris (Your digital co-driver)](https://www.hellochris.ai/)
by [German autolabs GmbH](https://germanautolabs.com)

### Current app state & challenges
- 💯% **Kotlin** (Yay!!!)
- **MVP** Architecture
- **Dagger** for Dependency injection
- **RxJava2** to wire-up different layers
- Separated **modules** as Google likes!
- **Firebase**
- one **MainService** background service that handles events (Calls, SMS, Calendar, eMail, and etc.)
- **Protobuf** protocol over BLE for smaller data transfer on BLE connection
- currently using [RxAndroidBle](https://github.com/Polidea/RxAndroidBle) library

### some BLE terms you can study more about here...
https://developer.android.com/guide/topics/connectivity/bluetooth-le.html
- **GATT:** The GATT profile is a general specification for sending and receiving short pieces of data known as "attributes" over a BLE link.
- **ATT:** GATT is built on top of the Attribute Protocol.
- **Characteristic:** A characteristic contains a single value and 0-n descriptors that describe the characteristic’s value.
- **Service:** A service is a collection of characteristics.
- **UUID!**

### Learnings from real world BLE
- BLE @ SumUp : 300k active installs 
- Murphy’s Law applies
- Disconnecting is your best friend
- #samsungwtf

### Device Discovery
- General Idea: Scan for Service UUID
- API Level < 21 : BluetoothAdapter.startLeScan()
  - :(
- API Level > 21: BluetoothAdapter.getBluetoothLeScanner()
  - Yay!
- But: Both have issues

### Advice on Scanning with 
- startLeScan() 
  - Do on main thread
  - Because #samsungwtf
  - Requires you to call stopLeScan()
  - Does not actually stop immediately
- getBluetoothLeScanner()
  - Do asynchronous (HandlerThread)
  - SCAN_FAILED_APPLICATION_REGISTRATION_FAILED 

### Making contact
- Connect to device UUID
  - Might not work    
  - Might not be your fault
  - Might be slow
    - Very slow
  - → Timeouts!
- Don’t confuse the stack
  - Async
  - Queue
  - Decouple
  - When in doubt, reconnect
  - Or even re-run discovery
  - Or... use a library :)
- Analytics!
  - Measure performance
  - Log to your backend
  - Create statistics
  - Analyze & Improve

### Questions (open mic):
- What're your experiences with Android BLE?
- What're considerations that we should take care of API below 21?

AFAIK there is a new background service limitation in Android Oreo:
- Is there any way to keep background service running on Android Oreo? (without using foreground services!!!)
- Could BLE wake up my app background service?
- Is there any suggestion for waking my service to serve user when he gets into the car and finds the Chris device?
