# Bing Chat Session Saver Bookmarklet

This bookmarklet allows you to save a copy of a Bing Chat session by opening the chat content in a new window. You can then view, save or share the content as needed.


## How to use
1. Copy the code block below and paste into a new browser bookmark named "Bing Chat Session Saver Bookmarklet".
2. Visit a Bing Chat page where you want to save the chat session.
3. Click on the bookmarklet in your bookmarks bar to run the script.
4. A new window or tab will open, displaying the chat session content in a nicely formatted HTML document.
5. You can now save the content or copy to your clipboard

## Code explanation

The bookmarklet code does the following:

* It uses the `querySelectorDeepMulti` function to traverse the shadow DOM and find the chat turn elements.
* The `createHTMLBlob` function concatenates the chat turn elements' outerHTML into a single HTML string, wraps it in a valid HTML structure with a title and a basic style for readability, and creates a Blob from the resulting HTML content.
* The `openBlobInNewTab` function creates a Blob URL and opens it in a new tab or window, displaying the chat session content.
* The code is wrapped in an Immediately Invoked Function Expression (IIFE) to avoid polluting the global namespace.


## Customization

If you wish to customize the appearance of the saved chat session, you can modify the CSS styles in the createHTMLBlob function. Simply edit the content within the <style> tags to change the styling of the chat messages or other elements.


## Disclaimer

This bookmarklet is provided as-is and without any warranties. Use it at your own risk. Always ensure you have permission to save and share chat content from any platform or service.

---



```js
javascript:(function () {
  function createHTMLBlob(turns) {
    const turnsHTML = turns.map((turn) => turn.outerHTML).join("\n");
    const htmlContent = `
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Chat Turns</title>
  <style>div.text-message-content { background: lightblue;}</style>
</head>
<body>
  ${turnsHTML}
</body>
</html>
  `;
    const blob = new Blob([htmlContent], {
      type: "text/html",
    });
    return blob;
  }

  function querySelectorDeepMulti(selectors, node = document) {
    const selector = selectors[0];
    const remainingSelectors = selectors.slice(1);

    const foundElements = Array.from(node.querySelectorAll(selector));
    if (remainingSelectors.length === 0) {
      return foundElements;
    }
    return foundElements.flatMap((e) =>
      querySelectorDeepMulti(remainingSelectors, e.shadowRoot)
    );

    return null;
  }

  function openBlobInNewTab(blob) {
    const url = URL.createObjectURL(blob);
    window.open(url, "_blank");
  }

  const turns = querySelectorDeepMulti([
    "cib-serp",
    "cib-conversation",
    "cib-chat-turn",
    "cib-message-group",
    "cib-message",
    "cib-shared div.content",
  ]);
  const htmlBlob = createHTMLBlob(turns);
  openBlobInNewTab(htmlBlob);
})();
```
