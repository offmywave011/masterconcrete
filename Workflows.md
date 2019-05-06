The **Workflow** Components are also part of the **RTU UI Components**. A Workflow represents a set of multiple scanning steps. You can combine Document Scanning with QR code detection or MRZ recognition on an ID card image, for example. Workflow Steps can be run on a captured still image or a video frame, so a step can either be a live-detection or a still-image capturing step. You can validate each step result, present an error message to the user if the validation fails and restart a step. By subclassing `WorkflowStep` the creation of custom steps is possible.

Following predefined Workflow Step classes are provided:
- `ScanDocumentPageWorkflowStep` - Specialized for capturing documents from high-res still images.
- `ScanBarCodeWorkflowStep` - Recognition of Barcodes/QR codes on low-res video frames (live detection).
- `ScanMachineReadableZoneWorkflowStep` - For scanning of ID cards or passports with MRZ recognition.
- `ScanDisabilityCertificateWorkflowStep` - For recognition and data extraction from Disability Certificates (DC) forms.
- `ScanPayFormWorkflowStep` - For recognition and data extraction from SEPA Payforms.

For more details please see the corresponding API docs of the classes as well as the example app [ready-to-use-ui-demo](https://github.com/doo/scanbot-sdk-example-android/tree/master/ready-to-use-ui-demo).
