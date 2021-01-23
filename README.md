# html2Canvas

## Description
Some of us may need to get a copy of the HTML page. It may not be as easy as it seems at first glance. I faced such a situation where I needed to copy a photo from the html page (https://www.textimage.com)
To do this, I created a small JavaScript that you just need to add to the browser console and call the command.

## How to use
How to use.
1. Open a website. I was using Firefox (84.0.2 (64-bit)
2. Open a Develope tool in the Browser.
3. Select Console tab
4. Pase javascript code
4.1. Adjust `htmlElementLocation` variable. It is selector on the page which you want to download.
5. Call `downloadAsImage()` method in console.

### Note
At the end, you should see a dialogue that will offer to save the image.
Note: Plese read about `html2canvas` script  on how to customize the image.

### JavaScript code
```js script
function downloadAsImage() {
  const imageFileName = "image.png";
  const htmlElementLocation =
    ".custom-block.custom-block__container-5.custom-block__masonry";

  async function loadScript(url) {
    let response = await fetch(url);
    let script = await response.text();
    eval(script);
  }

  if (typeof html2canvas !== "function") {
    let scriptUrl = "https://html2canvas.hertzen.com/dist/html2canvas.min.js";
    console.log(`Start loading script: ${scriptUrl}`);
    loadScript(scriptUrl).then((r) => {
      console.log(`Loaded script: ${scriptUrl}`);
      convertHtmlElementToCanvas();
    });
  } else {
    convertHtmlElementToCanvas();
  }

  function convertHtmlElementToCanvas() {
    let htmlElement = document.querySelector(htmlElementLocation);

    // (https://html2canvas.hertzen.com/configuration)
    html2canvas(htmlElement, {
      useCORS: true,
      allowTaint: true, // Whether to allow cross-origin images to taint the canvas
      backgroundColor: "rgba(0,0,0,0)", // transparent background
      scale: 2, // The scale to use for rendering. (get better quality) Defaults to the browsers device pixel ratio.
    }).then(function (canvas) {
      let imageData = canvas.toDataURL();
      let convertedData = imageData.replace(
        /^data:image\/png/,
        "data:application/octet-stream"
      );

      download(imageFileName, convertedData);
    });
  }

  function download(downloadFileName, hrefData) {
    let element = document.createElement("a");
    element.setAttribute("href", hrefData);
    element.setAttribute("download", downloadFileName);
    element.style.display = "none";

    document.body.appendChild(element);
    element.click();
    document.body.removeChild(element);
  }
}


```
