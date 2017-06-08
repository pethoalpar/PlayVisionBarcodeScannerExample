<h1>QR and barcode scanner example with play vision library, in android</h1>

<h2>Gradle</h2>

```gradle
 compile 'com.google.android.gms:play-services-vision:10.2.6'
 ```
 
 <h2>Implement the Detector.Processor interface</h2>
 
 ```java
 public class MainActivity extends AppCompatActivity implements Detector.Processor{
 ```
 
 <h2>Initialize variables</h2>
 
 ```java
BarcodeDetector barcodeDetector = new BarcodeDetector
 .Builder(this)
 .setBarcodeFormats(Barcode.ALL_FORMATS)
 .build();
barcodeDetector.setProcessor(this);

CameraSource cameraSource = new CameraSource.Builder(getApplicationContext(), barcodeDetector)
      .setRequestedPreviewSize(1024,768)
      .setAutoFocusEnabled(true)
      .build();
```

<h2>Start the camera service</h2>

```java
//I used a surface view to display the camera. 
cameraSource.start(surfaceView.getHolder());
```

<h2>Stop the camera service</h2>

```java
cameraSource.stop();
```

<h2>Obtain the value</h2>

```java
@Override
public void receiveDetections(Detector.Detections detections) {

  final SparseArray<Barcode> barcodes = detections.getDetectedItems();

  if(barcodes.size() != 0){
    final StringBuilder sb = new StringBuilder();
    for(int i =0 ; i<barcodes.size(); ++i){
      sb.append(barcodes.valueAt(i).rawValue).append("\n");
    }
    textView.post(new Runnable() {
      @Override
      public void run() {
        textView.setText(sb.toString());
      }
    });
  }
}
```
