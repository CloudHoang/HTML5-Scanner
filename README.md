# HTML5 Barcode Scanner for Power Apps

A modern, responsive HTML5-based barcode and QR code scanner designed specifically for integration with Power Apps. This iframe-compatible solution provides real-time scanning capabilities with a sleek, user-friendly interface.

## Features

- **Multi-Format Support**: Scans multiple barcode and QR code formats
  - QR Codes
  - Code 128, Code 39
  - EAN-13, EAN-8
  - UPC-A, UPC-E
  - ITF, PDF-417
  - Aztec, Data Matrix

- **Real-Time Scanning**: Continuous camera feed with live detection
- **Visual Feedback**: Animated scanner frame with laser effect and success confirmation
- **Responsive Design**: Works seamlessly on mobile, tablet, and desktop devices
- **Dark Theme UI**: Modern dark interface with smooth animations
- **Error Handling**: Graceful error states and permission management
- **Power Apps Integration**: Automatic communication via postMessage API
- **Accessibility**: ARIA labels and semantic HTML structure

## Usage

### Embedding in Power Apps

To use this scanner in Power Apps, embed it as a web resource in an iframe:

```html
<iframe src="[path-to-index.html]" style="width: 100%; height: 100%; border: none;"></iframe>
```

### Receiving Scanned Data

The scanner automatically sends scanned barcodes to the parent window using the postMessage API:

```javascript
window.addEventListener('message', function(event) {
  if (event.data.event === 'BarcodeScanned') {
    console.log('Scanned barcode:', event.data.value);
    // Process the barcode value
  }
});
```

## Technical Details

- **Scanner Library**: [html5-qrcode v2.3.8](https://unpkg.com/html5-qrcode@2.3.8/minified/html5-qrcode.min.js)
- **Camera Access**: Uses browser's native camera API with environment (rear) camera preference
- **No Dependencies**: Fully self-contained HTML file with inline CSS and JavaScript
- **Browser Support**: Chrome, Firefox, Safari, Edge (any modern browser with getUserMedia support)

## Features

### Status Indicators
- **Yellow dot**: Camera initializing
- **Green dot**: Ready to scan
- **Blue dot**: Processing scan
- **Red dot**: Camera error

### User Experience
- Animated scanning frame with "laser" effect
- Flash animation on successful scan
- Real-time status messages
- One-second pause between scans to prevent duplicate captures
- Responsive hints for mobile and desktop layouts

## Permissions

The scanner requires camera permissions to function:
- Users will be prompted by their browser to allow camera access
- Permission can be revoked in browser settings
- The scanner gracefully handles permission denial with clear error messages

## Scanning Tips

- **Optimal Distance**: 10-20 cm from the camera
- **Lighting**: Ensure adequate lighting for better detection
- **Angle**: Keep barcode/QR code perpendicular to camera
- **Camera Focus**: Allow a moment for autofocus to lock

## Browser Compatibility

| Browser | Support |
|---------|---------|
| Chrome/Edge | ✅ Full support |
| Firefox | ✅ Full support |
| Safari | ✅ Full support (iOS 11+) |
| Mobile Browsers | ✅ Full support |

## Security Notes

- This scanner uses the postMessage API with `*` origin. In production, consider restricting the target origin.
- Camera access is handled at the browser level with user consent
- No data is collected or sent to external servers

## Files

- `index.html` - Complete scanner application with embedded CSS and JavaScript

## License

Designed for Power Apps integration. Feel free to customize and modify for your specific needs.
