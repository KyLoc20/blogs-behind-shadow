# Write a Form (1): Understand its Functionality 

Basically, a Form consists of a `smart` container and its input elements. 

```html
<form>
    <input type="text" name="name" />
    <input type="email" name="email" />
    <input type="password" name="password" minlength="8" required>
    <input type="submit" value="Submit" />
</form>
``` 

## Overview

Usually *input elements* handle input from users while the *form container* do these things:

- Keeping mutable states and handle updates from input elements.
- Rendering a friendly form-like UI.
- Handling Validation before Submitting.
- Handling Submit event.

In the following parts, these 4 jobs above will be discussed.

Here is the simplest form component in React:

```typescript
function Form() {

    const [value1,setValue1] = useState("")
    const [value2,setValue2] = useState("")
    const [value3,setValue3] = useState("")

    const handleSubmit = (event) => {
        event.preventDefault();
        //do something mutating or uploading
    }
 
    return (
        <form onSubmit={handleSubmit}> 
            <InputElement1 value={value1} onChange={setValue1}/>
            <InputElement2 value={value2} onChange={setValue2}/>
            <InputElement3 value={value3} onChange={setValue3}/>
            <SubmitButton />
        </form>
    );
}
```

Benefiting from using `<form>`, this HTML tag provides us with some default behavior:

- Listening to *Submit Event* from its elements. `<input type="submit" />` or `<button type="submit" />` both could trigger this event so that you don't bother to define an extra event handler.
- Validating format of input data before `onSubmit` which is quite useful.
- Browsing to a new page defined in the attribute of `action`, however, often we need do more things in `handleSubmit` so block it by `event.preventDefault()`.

Next part let's talk about how to optimize our Form using the intrinsic *controlled* feature.