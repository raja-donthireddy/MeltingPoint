# MeltingPoint: Glossary

The domain words MeltingPoint uses: what each means, and nothing else. A defined term is written with capital initials wherever it is used in its defined sense.

## Language

### Temperatures and units

**Absolute Zero**:
The lowest possible Temperature: −273.15 °C, which is −459.67 °F. A Temperature exactly at Absolute Zero is valid; one below it is an Invalid Input.

**Celsius**:
One of the two Units, written °C. Water freezes at 0 °C.

**Conversion**:
Turning a Temperature in one Unit into the same Temperature in the other Unit, using the exact formula for that direction. A Conversion is never rounded; rounding belongs only to the Displayed Value.

**Fahrenheit**:
One of the two Units, written °F. Water freezes at 32 °F.

**Temperature**:
A numeric value together with its Unit, such as 37 °C. A bare number with no Unit is not a Temperature.

**Unit**:
The scale a Temperature is measured in: Celsius or Fahrenheit, and nothing else. Kelvin and other scales are not Units in this product.

### Fields and values

**Celsius Field**:
The Field that holds the Temperature in Celsius. It is the Source Field when the User last typed in it, and otherwise the Converted Field.

**Converted Field**:
The Field the User did not last type in. It shows the Converted Value, or is empty while the Source Value is an Invalid Input. It is a role, not a fixed Field.

**Converted Value**:
The result of a Conversion of a valid Source Value, shown in the Converted Field as a Displayed Value. A Converted Value exists only while the Source Value is valid; it is never a stale result kept from earlier input.

**Displayed Value**:
A Converted Value as the User sees it: rounded to 2 decimal places, with a space before the unit symbol, for example 98.60 °F. The Displayed Value is presentation only; the Conversion behind it is exact. The Source Value is never rewritten as a Displayed Value.

**Fahrenheit Field**:
The Field that holds the Temperature in Fahrenheit. It is the Source Field when the User last typed in it, and otherwise the Converted Field.

**Field**:
One of exactly two linked, labelled inputs: the Celsius Field and the Fahrenheit Field. Either Field can be typed in. Typing in one makes it the Source Field and the other the Converted Field.

**Live Update**:
The Converted Value is recalculated on every change to the Source Value, with no Convert action. A Live Update replaces the Converted Field's content, or clears it when the Source Value becomes an Invalid Input.

**Source Field**:
The Field the User last typed in. It is a role, not a fixed Field: either Field becomes the Source Field when typed in.

**Source Value**:
Exactly what the User has typed into the Source Field, before any Conversion. A Source Value is either a valid Temperature or an Invalid Input.

### Validity

**Invalid Input**:
A Source Value that cannot be converted: empty, not a number, or below Absolute Zero. An Invalid Input produces a Validation Message and no Converted Value. It is never shown as NaN.

**Validation Message**:
The plain, terse text shown beside the Source Field when its Source Value is an Invalid Input, for example "Enter a number." It names what is wrong in the User's terms. It is not a pop-up, a toast or an error code.

### People

**User**:
An anonymous person converting a Temperature in a web browser. A User has no account, login or session, and nothing about a User is stored or sent anywhere.
