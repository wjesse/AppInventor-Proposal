## MIT AppInventor Proposal - Jesse Wang

#### Part 2:

In order to let the camera take pictures automatically, the component would need to implement the android camera api, instead of depending on the default camera application of the device as it does now. 

The new component I would implement is the non-visible `AutoCamera`. It would have a method `autoPicture` which, when called, would automatically take a picture facing the current direction (front facing or back facing). 

AutoCamera would have the property `delay`, which enables the user to set a parameter for the delay between a method call to `autoPicture` and the actual picture being taken. It would also inherit any properties that the existing Camera component has.

##### Implementation:

To use the Android Camera API, we would need a new activity class `CameraHandler`, which would be defined in `appinventor-sources/appinventor/components/src/com/google/appinventor/components/runtime/CameraHandler.java`

- CameraHandler first checks if there is an available camera using the function:

    `context.getPackageManager().hasSystemFeature(PackageManager.FEATURE_CAMERA)`

- If such a camera exists, create an instance of a camera variable using Camera.open()

- In order for the user to actually use this component, we must create a temporary SurfaceView variable.

- Next use the Android SDK’s takePicture(ShutterCallback, PictureCallback, PictureCallback, PictureCallback) method as our takePicture method, implementing the PictureCallback and ShutterCallback as needed. 

- We can also implement a method that utilises the ShutterCallback to play a camera sound when the camera actually takes the photo, call this method playSound.


The AutoCamera.java component, which is located in the source tree at `appinventor-sources/appinventor/components/src/com/google/appinventor/components/runtime/AutoCamera.java` should extend `AndroidNonvisibleComponent`.

It has the property:
- `int delay`: default 0, `PropertyTypeConstants.PROPERTY_TYPE_INTEGER`

And has the methods:
- `void setDelay`: sets a value for `delay`
- `int getDelay`: gets the value for `delay`
- `String autoPicture`: takes a picture after `delay` seconds, returns the path of the picture

The `autoPicture` method creates a `CountDownTimer` object to count down from `delay` seconds, and uses the `CameraHelper.takePicture(...)` method to actually take the picture. It also uses `CameraHelper.playSound(...)` to play a camera shutter sound when the picture is taken.
