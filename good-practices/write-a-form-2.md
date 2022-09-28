# Write a Form (2): Self-hosted States

In HTML, form elements such as `<input>`, `<textarea>`, and `<select>` typically maintain their own state and update it based on user input.

We can exploit this feature. But fist thing first, talk about *controlled component*.

## Controlled Component

```tsx
function FreeInput({ defaultValue }: { defaultValue: string }) {
  const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
    console.log(e.target.value);
  };
  return <input type="text" onChange={handleChange} defaultValue={defaultValue} />;
```

It is always available to access the value of `<input>` from `e.target.value` of `onChange` because the value is *self-hosted*.

This `FreeInput` only expose the prop `defaultValue` to receive a initial value.

However it tends to combine this self-hosted value with a React state, by sending a state to `input.value` as a property:
 
```tsx
function ControlledInput({ defaultValue }: { defaultValue: string }) {
  const [value, setValue] = useState(defaultValue);
  const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
    console.log(e.target.value);
    setValue(e.target.value);
  };
  return <input type="text" value={value} onChange={handleChange} />;
}
```

In most situations, the state of `value` is sent from parent component as a prop:

```tsx
function FormAsController() {
  const [value1, setValue1] = useState("1");
  const [value2, setValue2] = useState("2");
  const [value3, setValue3] = useState("3");
  const handleSubmit = (e: FormEvent) => {
    e.preventDefault();
    console.log("Let's Submit: ", [value1, value2, value3]);
  };
  return (
    <form onSubmit={handleSubmit}>
      <InputControlledByParent value={value1} onChange={setValue1} />
      <InputControlledByParent value={value2} onChange={setValue2} />
      <InputControlledByParent value={value3} onChange={setValue3} />
      <button type="submit">Submit</button>
    </form>
  );
}

function InputControlledByParent({ value, onChange }: { value: string; onChange: (value: string) => void }) {
  const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
    console.log(e.target.value);
    onChange(e.target.value);
  };
  return <input type="text" value={value} onChange={handleChange} />;
}
```

Both `ControlledInput` and `InputControlledByParent` are called *controlled component* because their self-hosted values are controlled by React states.

This is to obey the principle of *"single source of truth"*, you can refer to [React Docs](https://reactjs.org/docs/forms.html#controlled-components).

## A Potential Problem

You may notice that putting `value` states of components `InputControlledByParent` into the component `FormAsController` could bring a *Re-Render* issue:

Once an `InputControlledByParent` updates its value, the whole `FormAsController` including all the `InputControlledByParent`s will get Rerendered.  

Oh, that's a little sick, especailly there are other components which should not be influenced.

Certainly we can wrap each `InputControlledByParent` with `React.useMemo`, but not today, I prefer to `React.useRef`.

## A Better Practice

Still remember we gonna use the feature of *self-hosted*? Here we are.

With `React.useRef`, each `Input` is allowed to host its own value and send its latest value to `Form` only when updates:

```tsx
function DemocraticForm() {
  const refValueList = useRef(["", "", ""]);
  const updateValueList = (index: number, value: string) => {
    refValueList.current[index] = value;
  };
  const handleSubmit = (e: FormEvent) => {
    e.preventDefault();
    console.log("Let's Submit: ", refValueList.current);
  };
  return (
    <form onSubmit={handleSubmit}>
      <SelfHostedInput defaultValue={"0"} onChange={(v) => updateValueList(0, v)} />
      <SelfHostedInput defaultValue={"1"} onChange={(v) => updateValueList(1, v)} />
      <SelfHostedInput defaultValue={"2"} onChange={(v) => updateValueList(2, v)} />
      <button type="submit">Submit</button>
    </form>
  );
}

function SelfHostedInput({ defaultValue, onChange }: { defaultValue: string; onChange: (value: string) => void }) {
  const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
    console.log(e.target.value);
    onChange(e.target.value);
  };
  return <input type="text" defaultValue={defaultValue} onChange={handleChange} />;
}
```

Now `SelfHostedInput` is truly *self-hosted*, and its update won't cause its `Form` to *re-render*.

> This way only works in the situation where the `Form`'s UI is NOT dependent on any input value.

Next part will talk about how to unify various input elements.