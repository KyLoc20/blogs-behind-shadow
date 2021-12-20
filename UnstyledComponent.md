# Unstyled Component: a stepping stone to UI building

An Unstyled Component is a free-to-style Component keeping only the core functionality.

## Simple Practices

Image you have wrote a Button Component with some customized styles such as round borders, a background color of #007FFF, a hovering content color of #1867c0, specific height&width&padding and blablabla, what if you wanna reuse this Button with some nuanced changes?
Easily you define some Props for your Button:

```typescript
type ButtonProps = {
  width?: string;
  height?: string;
  padding?: string;
  backgroundColor?: string;
  hoverBackgroundColor?: string;
  borderRadius?: string;
};
```

You may define some "sugar" props in order to save yourself from parsing these styte props in the implementation.

```typescript
type ButtonProps = {
  round?: boolean; // "sugar" for borderRadius
  borderRadius?: number | string;
  size?: "sm" | "md" | "lg"; // "sugar" for height&padding,
  height?: string;
  padding?: string;
};
function MyButton(props: ButtonProps) {
  let styleBorderRadius = props.round ? "50%" : props.borderRadius;
  let styleHeight, stylePadding;
  switch (props.size) {
    case "sm":
      styleHeight = "28px";
      stylePadding = "0 12px";
    case "sm":
      styleHeight = "36px";
      stylePadding = "0 16px";
    case "sm":
      styleHeight = "44px";
      stylePadding = "0 20px";
    default:
      styleHeight = "28px";
      stylePadding = "0 12px";
  }
  return (
    <Button
      style={{
        borderRadius: styleBorderRadius,
        height: styleHeight,
        padding: stylePadding,
      }}
    >
      {props.children}
    </Button>
  );
}
```

## Trouble

You could find many such "sweet" props from almost every UI libs. Let me call them LocalCustomProps and they are useful with no doubts.
However, pain comes into you when you are required to custom more and more style props like cursor, font-size, flex-direction, align-items, box-shadow... You have to put almost all the css properties into your Button.
More deadly, you are expected to build another component Text and do that porter job again.
Here comes the idea of extracting css props from components and only keeping the core features. Let me call them GenericCustomProps or sxProps.

## What matters to a Component?

Before diving into the sxProps, let's think it about that what is the incentive of designing the component Button.

- I need a box which contains some prompt icons or text
- I need this box to be clickable and it triggers some events when clicked
- I wanna add some fancy features to make my Button more unique such as a ripple effect when clicked

Besides these maybe you plan to decorate this component with a background color, a box shadow and other styles to make it more touchable. The Button is a Box essentially apart from the features above so we can endow all the styles to it under the premise that the core feature styles must not be overrided.

## sxProps
