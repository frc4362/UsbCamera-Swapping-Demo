![Gems Robotics](https://gemsrobotic.weebly.com/uploads/7/7/8/2/77822928/editor/yeetskeet.png?1520219568)

**Swapping Between Multiple UsbCameras**

Doing this the super simple way leaves some delay time between camera views, since as soon as a camera is not actively being polled, it will be freed by the program. But this is easy to remedy by attaching a `CvSink` to any `UsbCamera` we keep ownership even if we are not posting frames to `CameraServer`.

To aid in this I've included a `OwnedCamera` class in this repository which lets you wrap a `UsbCamera` instance with an empty and automatically managed `CvSink` to remove the the time it takes to reacquire the camera. I've only done this in Java but this all applies to other languages- you'll still have to write those bindings yourself.

That said, this operation is still neither free nor memoized- if you use `CameraServer#startAutomaticCapture` constantly in your code, you will still experience lag or CPU usage issues.

Constructing a new `OwnedCamera` is simply a matter of passing an existing instance of `UsbCamera`, it will manage the `CvSink` for you. After this I recommend using `CameraServer#startAutomaticCapture(VideoSource)` to change to the `UsbCamera` instance whenever you want to swap. After this just add your `CameraServer` viewing widget to SmartDashboard or Shuffleboard as you see fit. 

Endorsed methods for swapping views are included here
- Using methods on `Trigger` or on `Button` in a command based architecture
- Saving the value of your button and monitoring for changes, and updating the selected camera as necessary. 

Feel free to message me at `ejmalzone@gmail.com` or `Ethab#4362` on Discord. -  Ethan ^_^
