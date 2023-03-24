Copy and apply in a Bing chat session

```js
javascript: (function () {
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
