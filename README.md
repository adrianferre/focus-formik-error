# Focus Formik error

This component allows you to automatically focus errors in your [Formik](https://formik.org/docs/overview) form, the focus runs after the [validation](https://formik.org/docs/guides/validation#when-does-validation-run).

Main features
- Support nested Arrays, nested Objects and both combined in values.
- Simple and lightweight only 24kb, no extra animations.
- No dependencies required.

![Example](https://github.com/adrianferre/focus-formik-error/blob/main/autoFocusExample.gif?raw=true)

## Getting Started

```js
npm i focus-formik-error
```

## How to use it

You can use it by including it inside the Formik provider or also use ti with useFormik hook as follow.

### With Formik provider

Include the `<ConnectedFocusError />` component inside the Formik context provider.

```tsx
import React from "react";
import { Formik, Form, Field, ErrorMessage } from "formik";

import { ConnectedFocusError } from "focus-formik-error";

const Basic = () => (
  <Formik
    initialValues={{
      name: "Poseidon",
    }}
    onSubmit={(values, { setSubmitting, setErrors }) => {
      // Simulates server side validation
      setTimeout(() => {
        const errors = {} as any;

        if (!values.name) {
          errors.country = "Name is required";
        }

        setErrors(errors);
        setSubmitting(false);
      }, 500);
    }}
  >
    {({ errors, isSubmitting }) => (
      <Form className={"form"}>
        <ConnectedFocusError />
        <div className={"input-container"}>
          <label>Name </label>
          <Field
            type="text"
            name="name"
            className={`input ${errors.name ? "input-error" : ""}`}
          />
          <ErrorMessage name="name" component="div" className={"error-text"} />
        </div>
        <button
          type="submit"
          disabled={isSubmitting}
          style={{ marginBottom: "5em" }}
        >
          {isSubmitting ? "Submitting" : "Submit"}
        </button>
      </Form>
    )}
  </Formik>
);

export default Basic;

```

### With useFormik

Include the `<FocusError />` component inside you form and pass `formik` as props.

> **NOTE: Follow the same pattern in the name attribute of the input component used in the `initialValues` or the focus is not going to work. I.g: `name="values.name"`**

```tsx
import React from "react";
import { useFormik } from "formik";

import { FocusError } from "focus-formik-error";

const UseFormikExample = () => {
  const formik = useFormik({
    initialValues: {
      name: "",
    },
    onSubmit: (values, { setSubmitting, setErrors }) => {
      setTimeout(() => {
        // Simulates server side validation
        const errors = {} as any;

        if (!values.name) {
          errors.country = "Name is required";
        }

        setErrors(errors);
        setSubmitting(false);
      }, 400);
    },
  });

  return (
    <form onSubmit={formik.handleSubmit}>
      <FocusError formik={formik} />
      <input
        id="values.name"
        name="values.name"
        type="text"
        onChange={formik.handleChange}
        value={formik.values.name}
      />
      {formik.errors.name}
      <button type="submit" disabled={formik.isSubmitting}>
        Submit
      </button>
    </form>
  );
};

export default UseFormikExample;
```

### Options

| Prop           | Type          | Default  | Description  |
| -------------- | ------------- | -------- | -------- |
| `focusDelay`   | number (ms)       | 100      | Time in ms to execute the focus in the error component |

## Contribute

I actively welcome pull requests for improvements or fixes.

## License

[MIT License](./LICENSE.md)

