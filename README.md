
# Image to PDF Converter

This script converts all images (PNG and JPG) in a specified directory into a single PDF file.

## Requirements

- Node.js
- NPM (Node Package Manager)

## Installation

1. Clone this repository or download the files.
2. Navigate to the project directory in your terminal.
3. Install the necessary dependencies by running:

    ```bash
    npm install pdfkit
    ```

## Usage

1. Make sure you have a directory named `input` in the root of your project, and place the images (PNG and JPG) you want to convert into this directory.

2. Run the script with the following command:

    ```bash
    node script.js <output_file_name>
    ```

    Replace `<output_file_name>` with the desired name for the output PDF file. The file will be saved in an `out` subdirectory with the `.pdf` extension added automatically.

    Example:

    ```bash
    node script.js my_document
    ```

    This will generate a PDF file named `my_document.pdf` in the `out` directory.

## Code

```javascript
const PDFDocument = require('pdfkit');
const path = require('path');
const fs = require('fs');

// Arguments
if (process.argv.length < 3) {
  console.error('Usage: node script.js <output_file_name.pdf>');
  process.exit(1);
}

// Params
const pdfFilePath = `out/${process.argv[2]}.pdf`;
const pngDir = './input/'; 

const doc = new PDFDocument();
const writeStream = fs.createWriteStream(pdfFilePath);
doc.pipe(writeStream);

// Generate PDF document
fs.readdir(pngDir, (err, files) => {
  if (err) {
    return console.error(`Could not read the directory: ${err.message}`);
  }

  const imageFiles = files.filter(file => ['.png', '.jpg'].includes(path.extname(file).toLowerCase()));

  imageFiles.forEach((imageFile, index) => {
    if (index > 0) {
      doc.addPage();
    }

    const imagePath = path.join(pngDir, imageFile);

    doc.image(imagePath, {
      fit: [500, 700], 
      align: 'center',
      valign: 'center'
    });
  });

  doc.end();
});

writeStream.on('finish', () => {
  console.log(`PDF file created: ${pdfFilePath}`);
});
```

## Notes

- Make sure the `out` directory exists in the root of your project. If it doesn't, create it before running the script.
- This script only processes files with `.png` and `.jpg` extensions.

## Contributions

Contributions are welcome. Please open an issue or a pull request for any improvements or corrections.

## License

This project is licensed under the [MIT License](LICENSE).
