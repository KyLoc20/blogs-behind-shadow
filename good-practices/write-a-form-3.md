# Write a Form (3): Unified Input 

Everyday we write various input elements to deal with various types like string, number and boolean.

Then we combine them in one component and call it a form component.

Where there is a variety, there is a complexity.

This part talks about that complexities should be placed IN the *input components* NOT OUT of them.

## Messy Input

Suppose we have a form component with 3 input elements:

```tsx
function MySimpleFormComponent() {
  const refValueList = useRef<any[]>(["", "1", true]);
  const handleChange = (index: number, e: ChangeEvent<HTMLInputElement>) => {
    const updateValueList = (index: number, value: any) => {
      refValueList.current[index] = value;
    };
    updateValueList(index, e.target.type === "checkbox" ? e.target.checked : e.target.value);
  };
  const handleSubmit = (e: FormEvent) => {
    e.preventDefault();
    console.log("Let's Submit: ", refValueList.current);
  };
  return (
    <form onSubmit={handleSubmit}>
      <input type="text" onChange={(e) => handleChange(0, e)} defaultValue={"default"} />
      <input type="number" onChange={(e) => handleChange(1, e)} defaultValue={"1"} />
      <input type="checkbox" onChange={(e) => handleChange(2, e)} defaultChecked={true} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

Using `<input>` directly brings issues that make us feel bad:
- NOT unified types: `defaultValue` and `defaultChecked`
- NOT unified handlers: `e.target.checked` and `e.target.value`
- limited functionalities and validations
- messy JSX structures

## Unified Component

Eagerly we need clean and tidy *Input Component*s:

```tsx
function TextInputComponent({ defaultValue, onChange }: { defaultValue: string; onChange: (value: string) => void }) {
  //custom implementation
  const [value, setValue] = useState(defaultValue);
  useEffect(() => {
    onChange(value);
  }, [value]);
  return <div>Your Custom TextInputComponent: {defaultValue}</div>;
}
function NumberInputComponent({ defaultValue, onChange }: { defaultValue: number; onChange: (value: number) => void }) {
  //custom implementation
  // ...
  return <div>Your Custom NumberInputComponent: {defaultValue}</div>;
}
function CheckboxInputComponent({ defaultValue, onChange }: { defaultValue: boolean; onChange: (value: boolean) => void }) {
  //custom implementation
  // ...
  return <div>Your Custom CheckboxInputComponent: {defaultValue}</div>;
}
```

Each InputComponent has its own states, styles, logic and so on. 

They could be implemented as various utilities while be treated as unified `input` elements because they have props `onChange` and `defaultValue` in common.

## A Better Practice

Let's write a better form:

```tsx 
function MyBetterFormComponent() {
  const refValueList = useRef<any[]>(["", 1, true]);
  const handleChange = (index: number, value: any) => {
    const updateValueList = (index: number, value: any) => {
      refValueList.current[index] = value;
    };
    updateValueList(index, value);
  };
  const handleSubmit = (e: FormEvent) => {
    e.preventDefault();
    console.log("Let's Submit: ", refValueList.current);
  };
  return (
    <form onSubmit={handleSubmit}>
      <TextInputComponent onChange={(v) => handleChange(0, v)} defaultValue={"default"} />
      <NumberInputComponent onChange={(v) => handleChange(1, v)} defaultValue={1} />
      <CheckboxInputComponent onChange={(v) => handleChange(2, v)} defaultValue={true} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

> Remember: consume `<InputComponent>`s as if they are `<input>`s.

Next part will talk about how to customize a useful validator inside a `<InputComponent>`.