The `signal` package in Python provides mechanisms to handle asynchronous events by allowing you to set handlers for system signals. Signals are used by the operating system to notify your program of events like interrupts (Ctrl+C), alarms, or termination requests.

Here are common uses of the `signal` package:

### 1. **Handling Interrupts (Ctrl+C)**
- You can capture keyboard interrupts (e.g., Ctrl+C) and handle them gracefully, preventing abrupt program termination.
   ```python
   import signal
   import sys

   def handler(signum, frame):
       print("Signal handler called with signal:", signum)
       sys.exit(0)

   signal.signal(signal.SIGINT, handler)

   while True:
       pass  # Run an infinite loop until Ctrl+C is pressed
   ```

### 2. **Setting Alarms**
- The `signal.alarm()` function allows you to schedule a signal to be delivered after a certain number of seconds. This is useful for setting timeouts in long-running operations.
   ```python
   import signal
   import time

   def handler(signum, frame):
       print("Timeout! Signal received:", signum)
       raise TimeoutError("Operation timed out")

   signal.signal(signal.SIGALRM, handler)
   signal.alarm(5)  # Trigger SIGALRM after 5 seconds

   try:
       time.sleep(10)  # Simulate a long-running operation
   except TimeoutError as e:
       print(e)
   ```

### 3. **Handling Termination Signals**
- You can capture termination signals like `SIGTERM` to clean up resources (like closing files, releasing locks) before your program terminates.
   ```python
   import signal
   import sys

   def termination_handler(signum, frame):
       print("Termination signal received. Cleaning up...")
       sys.exit(0)

   signal.signal(signal.SIGTERM, termination_handler)

   while True:
       pass  # Keep running until termination signal is received
   ```

### 4. **Ignoring Signals**
- You can choose to ignore specific signals, preventing them from interrupting the program.
   ```python
   signal.signal(signal.SIGINT, signal.SIG_IGN)  # Ignore Ctrl+C (SIGINT)

   while True:
       pass  # The program won't terminate with Ctrl+C
   ```

### 5. **Custom Signal Handlers**
- Custom signal handlers can be defined for specific signals. For example, you might handle `SIGUSR1` and `SIGUSR2`, which are user-defined signals.
   ```python
   import signal

   def custom_signal_handler(signum, frame):
       print(f"Custom signal {signum} received")

   signal.signal(signal.SIGUSR1, custom_signal_handler)
   signal.signal(signal.SIGUSR2, custom_signal_handler)

   while True:
       pass  # Wait for the signals to trigger
   ```

### 6. **Pausing Execution Until a Signal**
- The `signal.pause()` function allows the program to wait until any signal is received.
   ```python
   import signal

   print("Waiting for a signal...")
   signal.pause()  # Pause execution until a signal is received
   ```

### 7. **Signal Blocking**
- You can block certain signals temporarily using `signal.sigprocmask()`. This is useful in multi-threaded programs where signals should only be handled by the main thread.

   ```python
   import signal

   blocked_signals = signal.SIGINT  # Block Ctrl+C temporarily
   signal.pthread_sigmask(signal.SIG_BLOCK, [blocked_signals])

   # Do some critical operations here...

   signal.pthread_sigmask(signal.SIG_UNBLOCK, [blocked_signals])  # Unblock Ctrl+C
   ```

### Summary of Common Signals
- `SIGINT`: Interrupt signal (Ctrl+C)
- `SIGTERM`: Termination signal
- `SIGALRM`: Alarm signal, raised after a set duration
- `SIGUSR1` and `SIGUSR2`: User-defined signals
- `SIGHUP`: Hangup signal (sent when a controlling terminal is closed)

The `signal` package is particularly useful for writing programs that need to handle asynchronous events, like system interrupts or timeouts, allowing for cleaner, more controlled program termination or behavior.