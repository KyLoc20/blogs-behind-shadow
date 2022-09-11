# Unstyled Component: a Stepping Stone To UI Building

An Unstyled Component is a free-to-style Component keeping only the core functionality.

## Simple Practices

Image you have wrote a Button Component with some customized styles such as round borders, a background color of #007FFF, a hovering content color of #1867c0, specific height+width+padding...

What if you wanna reuse this Button with some nuanced changes? Easily you define some Props for your Button:

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

However, pain comes into you when you are required to custom more and more style props like cursor, font-size, flex-direction, align-items, box-shadow... You have to put almost all the CSS properties into your Button.

More deadly, you are expected to build another component Text with many style custom demands then you do that porter job again.

Here comes the idea of extracting CSS props from components and only keeping the core features. Let me call them GenericCustomProps or sxProps.

## What matters to a Component?

Before diving into the sxProps, let's think it about that what is the incentive of designing the component Button.

- I need a box which contains some prompt icons or text
- I need this box to be clickable and it triggers some events when clicked
- I wanna add some fancy features to make my Button more unique such as a ripple effect when clicked

Besides these maybe you plan to decorate this component with a background color, a box shadow and other styles to make it more touchable.

The Button is a Box essentially apart from the features above so we can endow all the styles to it under the premise that the core feature styles must not be overrided.

## sxProps

This is a concept from [MUI](https://mui.com/system/the-sx-prop/) which is originally a shortcut for defining custom style that has access to the theme while I suppose it as Style miXtrue Props.

In my Inplementation of sxProps, I include CSS properties as many as possible. See them [here](https://github.com/KyLoc20/twitter-shadow/blob/master/system/sx.tsx).

Basically a factory function is provided with the help of [emotion](https://github.com/emotion-js/emotion) CSS-in-JS support].styled.

```typescript
import styled, { StyledComponent } from "@emotion/styled";
type sxProps = SpacingProps &
  PaperProps &
  SizingProps &
  PositionProps &
  TextProps &
  DisplayProps &
  FlexProps;

type SpacingProps = {
  m?: string; //margin
  mt?: string; //margin-top
  mr?: string; //margin-right
  mb?: string; //margin-bottom
  ml?: string; //margin-left
  mx?: string; //margin-left&margin-right
  my?: string; //margin-top&margin-bottom
  p?: string; //padding
  pt?: string; //padding-top
  pr?: string; //padding-right
  pb?: string; //padding-bottom
  pl?: string; //padding-left
  px?: string; //padding-left&padding-right
  py?: string; //padding-top&padding-bottom
};

//other kinds of CSS properties skipped

function createUnstyleComponent<T extends sxProps>(
  baseComponent: StyledComponent<{}>
) {
  //other style parsers skipped
  return styled(baseComponent)`
    margin: ${(props: T) => props.m};
    margin-top: ${(props: T) => props.my || props.mt};
    margin-right: ${(props: T) => props.mx || props.mr};
    margin-bottom: ${(props: T) => props.my || props.mb};
    margin-left: ${(props: T) => props.mx || props.ml};
    padding: ${(props: T) => props.p};
    padding-top: ${(props: T) => props.py || props.pt};
    padding-right: ${(props: T) => props.px || props.pr};
    padding-bottom: ${(props: T) => props.py || props.pb};
    padding-left: ${(props: T) => props.px || props.pl};

    &:hover {
      color: ${(props: T) => props.hoverColor};
      background: ${(props: T) => props.hoverBg};
    }
  `;
}
```

As you can see, some shortcuts for property names are used like "m"("margin"), "p"("padding"), "w"("weight")...

## How to use

Without sxProps, you have write components with styles one by one:

```typescript
import styled, { StyledComponent } from "@emotion/styled";

const Wrapper = styled("div")`
  margin: 20px;
  min-width: 300px;
`;
const InnerContent = styled("div")`
  display: flex;
  flex-direction: column;
  border: 1px solid rgb(239, 243, 244);
  background: rgba(0, 0, 0, 0.2);
  border-radius: 4px;
  padding: 5px 12px;
`;
const Text = styled("div")`
  font-size: 24px;
  line-hegiht: 30px;
  color: red;
  &:hover {
    color: green;
  }
`;
function ExampleWithoutSxProps() {
  return (
    <Wrapper>
      <InnerContent>
        <Text>123</Text>
        <Text>456</Text>
        <Text>789</Text>
      </InnerContent>
    </Wrapper>
  );
}
```

with sxProps, you don't bother to define and name you components just using an UnstyledBox:

```typescript
import styled from "@emotion/styled";
import { sxProps, createUnstyleComponent } from "@/system/sx";
//define styles in common here
const BasicBox = styled.div`
  font-family: "Roboto";
`;
const Box = createUnstyleComponent<sxProps>(BasicBox);
function ExampleUsingSxProps() {
  return (
    <Box {...{m:"20px", minWidth:300}}>
      <Box {...{display:"flex",flexDirection:"column",border:"1px solid rgb(239, 243, 244)",bg:"rgba(0, 0, 0, 0.2)",
      borderRadius:4,p:"5px 12px"}}>
        <Box {...{fontSize:24,lineHeight:30,color:"red",hoverColor:"green"}}>123</Box>
        <Box {...{fontSize:24,lineHeight:30,color:"red",hoverColor:"green"}}>456</Box>
        <Box {...{fontSize:24,lineHeight:30,color:"red",hoverColor:"green"}}>789</Box>
      </Box>
    </Box>
})
```

As you can see, styles is defined as objects which allows you to manipulate them more freely.

## Further Inprovements

- I still feel it cumbersome that components receive style object as props in JSX -> to seperate styles from JSX
- I still wanna name each of components in the layouts -> to name components individually or not
- I wish to keep my custom props after naming/defining and reuse them -> to provide reusable custom components

Some hooks are written to make these improvements above which is introduced in the next blog.
