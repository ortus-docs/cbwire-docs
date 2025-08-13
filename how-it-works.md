# How It Works

This diagram shows the Counter we created in the [Introduction](./) and how CBWIRE re-renders our template and updates the DOM.

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

1. The user requests a page from the server that includes our Counter component.
2. ColdBox receives the request.
3. CBWIRE parses our component and initially renders it with its default values.&#x20;
4. The rendered component is sent to the browser.\
   &#xNAN;_**It's important to understand that nothing extraordinary has happened at this point. We've returned HTML to the browser from an incoming request. Next is when things get interesting.**_
5. The user clicks the button.
6. CBWIRE uses Livewire.js to intercept the user's click on the client side and send an XHR request to the server.
7. CBWIRE runs the increment() method, updating the data properties.
8. CBWIRE re-renders the template.
9. CBWIRE returns the re-rendered template to the browser.
10. CBWIRE uses Livewire.js DOM diffing technology to update the component on the page.
