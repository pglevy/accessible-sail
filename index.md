---
title: Home
---

# Appian SAIL Accessibility Checklist and Examples

How to use this reference:
* Review the checklist when creating SAIL interfaces.
* Use examples as starting points for incorporating best practices.
* Ask questions if you come across something you’re not sure about.

On this page:
* [Inputs and Controls](#inputs-and-controls)
* [Content](#content)
* [Tips](#tips)
* [Examples](#examples)

## Inputs and Controls

* All form inputs and controls have labels (even if hidden)
  1 Both individual inputs like `a!textField` and multi-item inputs like `a!radioButtonField`
  * Don’t use placeholder text as a substitute for label
* Make labels visible unless there is a functional reason to hide them (e.g., inline fields for a sentence format)
* Input purpose is set if known
* Required fields are marked as required, even if they can’t be unselected or null
* Use the `validations` or `validationGroup` parameter for input error messages
* When instructions are needed, use the component `instructions` parameter when possible to include them with keyboard focus
  * Don’t duplicate other visual output, including tooltips
  * Don’t use placeholder text as a substitute for instructions
* Do not use links, buttons, and other controls inside `a!cardLayout` when used as a link (nested controls)
* Use out-of-the-box pagination when possible
* When using duplicate control names (e.g., a repeated download icon link or show more button), set unique `accessibilityText` for each one so the distinct action is clear. This does not apply to grids.
* Use `accessibilityText` to indicate selected state or other context for non-Selection components (e.g., selected tab or button)

## Content

* Provide alternative text for images that convey information.
  * Use `caption` for icons not universally known (shows tooltip).
  * Use `altText` for icons universally known, like search (no tooltip but read by screen reader).
  * `altText` and `caption` may both be used if they convey different information, but should not be duplicated.
  * Don’t include alternative text for decorative images or icons.
  * Provide alternate grid views for charts when possible.
* Use `accessibilityText` parameter only to provide context that is not otherwise available (e.g., reminding what section an input is in).
* Use `a!richTextHeader`, `a!sectionLayout`, or `a!boxLayout` with the [appropriate size](https://docs.appian.com/suite/help/23.4/sail/ux-accessibility.html#use-accessible-headers) when possible to achieve semantic heading elements.
* Use `a!richTextBulletedList` or `a!richTextNumberedList` for semantic list content.
* Use out-of-the-box grids as much as possible (instead of recreating something that looks like a grid).
  * Use column headers (unless not needed for icons or icon buttons).
  * For grids, use the `rowHeader` parameter to indicate which column is used to identify the row content.
  * Include `accessibilityText` for read-only grids with [conditionally-formatted background colors](https://docs.appian.com/suite/help/23.4/Grid_Column_Component.html#using-the-backgroundcolor-parameter) to explain the significance of each background color.
* Avoid using labels on rich text (unless needed for consistency between rich text items and other input fields).
* Use as few tooltips as possible on a given page and keep them brief.
* Do not rely on color to convey information; include text or other visual changes along with the color to represent meaning.
* Ensure text on backgrounds and adjacent objects meet contrast requirements for [text](https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum) and [non-text](https://www.w3.org/WAI/WCAG21/Understanding/non-text-contrast.html).
* Pages are responsive and work well at high zoom levels or on smaller devices.

## Tips
* Use out-of-the-box components and functionality when possible.
* [Be cautious with tooltips](https://docs.google.com/presentation/d/1R3YSrCDeX9JS4K3PTbkrZXDHGZuymknt_jNiw3bex5Q/edit#slide=id.g13cf1d53424_0_47). (Internal Appian content)
* [Be cautious with accessibility text](https://docs.google.com/presentation/d/1R3YSrCDeX9JS4K3PTbkrZXDHGZuymknt_jNiw3bex5Q/edit#slide=id.g13cf1d53424_0_55). (Internal Appian content)
* Use Safari with VoiceOver for testing on a Mac.

## Examples

To test out these SAIL snippets, you copy and paste them into the code editor on [this page](https://docs.appian.com/suite/help/24.1/sail/Title_Bar_Header.html) and press `EVALUATE`. Some examples use internal-only functions and may not render.

### Inputs

#### Text field (standard)

* Always includes label.
* Avoid placeholder text.

```
a!textField(
  label: "Email",
  labelPosition: "ABOVE",
  inputPurpose: "EMAIL",
  instructions: "Remember to check your spam folder",
  required: true
)
```

#### Text field (custom)
* If visual label is provided using rich text, label still included on input.

```
a!sideBySideLayout(
  items: {
    a!sideBySideItem(
      item: a!richTextDisplayField(
        labelPosition: "COLLAPSED",
        value: a!richTextItem(text: "Full name", size: "MEDIUM"),
        marginBelow: "NONE",
        preventWrapping: true
      ),
      width: "MINIMIZE"
    ),
    a!sideBySideItem(
      item: a!textField(
        label: "Full name",
        labelPosition: "COLLAPSED",
        saveInto: {},
        refreshAfter: "UNFOCUS",
        validations: {},
        
      )
    )
  },
  alignVertical: "MIDDLE",
  spacing: "SPARSE"
)
```

#### Checkbox (multi-item inputs)
* Inputs with multiple items get their own label too.

```
a!checkboxField(
  choiceLabels: {
    "Honey bee",
    "Bumblebee",
    "Carpenter bee"
  },
  choiceValues: { 1, 2, 3 },
  label: "Bees",
  labelPosition: "ABOVE",
)
```

#### Checkbox (single checkbox selection)
* When using a checkbox field for a single selection, where the label is the choice label, a label is not included unless it provides useful context in addition to the choice label.

```
a!checkboxField(
  label: null,
  labelPosition: "COLLAPSED",
  choiceValues: true,
  choiceLabels: "I agree to these terms",
  required: true
)
```

#### Validation message

```
a!localVariables(
  local!value: "me@emailcom",
  a!textField(
    label: "Email",
    labelPosition: "ABOVE",
    inputPurpose: "EMAIL",
    instructions: "Remember to check your spam folder",
    required: true,
    value: local!value,
    validations: "Please enter a valid email address"
  )
)
```

### Buttons

#### Button with redundant image
* Does not include `accessibilityText`.

```
a!buttonArrayLayout(
  buttons: {
    a!buttonWidget(
      icon: "search",
      label: "Search",
    )
  }
)
```

#### Button with meaningful image
* Includes `accessibilityText` description of image.

```
a!buttonArrayLayout(
  buttons: {
    a!buttonWidget(
      icon: "folder",
      label: "Search",
      accessibilityText: "Folder"
    )
  }
)
```

#### Icon Button
* Includes `accessibilityText` in lieu of label.

```
a!buttonArrayLayout(
  buttons: {
    a!buttonWidget(
      icon: "search",
      style: "OUTLINE",
      accessibilityText: "Search records"
    )
  }
)
```

### Cards
#### Linked cards
* When linking cards, note there are no other controls inside the card.

```
a!cardLayout(
  contents: a!richTextDisplayField(
    labelPosition: "COLLAPSED",
    value: {
      a!richTextHeader(text: "A Special Button"),
      a!richTextItem(text: "For when regular buttons just won’t do")
    },
    align: "CENTER"
  ),
  shape: "ROUNDED",
  link: a!dynamicLink(value: true)
)
```

### Visual Content
#### Images

```
a!imageField(
  labelPosition: "COLLAPSED",
  images: a!webImage(
    source: "https://docs.appian.com/suite/help/23.4/images/design-sys/sail_welcome_illustration.png",
    altText: "Abstract illustration of two people collaboratively building an interactive website"
  )
)
```

#### Icons (Not Universally Known)
* Use caption, not tooltip on `a!richTextDisplayField`.

```
a!richTextDisplayField(
  labelPosition: "COLLAPSED",
  value: {
    a!richTextIcon(
      icon: "brain",
      caption: "Artificial Intelligence"
    )
  }
)
```

#### Icons (Universally Known)
* Use `altText`.

```
a!richTextDisplayField(
  labelPosition: "COLLAPSED",
  value: {
    a!richTextIcon(
      icon: "search",
      altText: "Search"
    )
  }
)
```

### Lists
#### Bulleted Lists

```
a!richTextDisplayField(
  labelPosition: "COLLAPSED",
  value: {
    a!richTextBulletedList(
      items: {
        "First",
        "Second",
        "Third",
        a!richTextItem(text: "Fourth", style: "EMPHASIS"),
        "Fifth"
      }
    )
  }
)
```

#### Numbered Lists

```
a!richTextDisplayField(
  labelPosition: "COLLAPSED",
  value: {
    a!richTextNumberedList(
      items: {
        "First",
        "Second",
        "Third",
        a!richTextItem(text: "Fourth", style: "EMPHASIS"),
        "Fifth"
      }
    )
  }
)
```

### Grids
#### Simple Grid
* Note there is currently a bug that ignores `backgroundColor` when `rowHeader` is used.

```
a!localVariables(
  local!data: todatasubset(
    arrayToPage: {
      a!map(
        name: "Gray Light",
        hex: "#F0F0F0",
        group: "UI"
      ),
      a!map(
        name: "Charcoal Mercury Highlight",
        hex: "#99A8C5",
        group: "Theme"
      ),
      a!map(
        name: "Info Foreground",
        hex: "#2C5791",
        group: "State"
      ),
      
    }
  ).data,
  {
    a!gridField(
      label: "Colors",
      labelPosition: "ABOVE",
      data: local!data,
      columns: {
        a!gridColumn(
          label: "Name",
          sortField: "name",
          value: fv!row.name,
          backgroundColor: if(fv!identifier = 3, "#ECF4FF", {}),
          accessibilityText: if(fv!identifier = 3, "New", {})
        ),
        a!gridColumn(
          label: "Hex",
          sortField: "hex",
          value: fv!row.hex,
          backgroundColor: if(fv!identifier = 3, "#ECF4FF", {})
        ),
        a!gridColumn(
          label: "Group",
          sortField: "group",
          value: fv!row.group,
          backgroundColor: if(fv!identifier = 3, "#ECF4FF", {})
        )
      },
      rowHeader: 1
    )
  }
)
```

### Patterns
#### Breadcrumbs
* Add “Breadcrumb” as `accessibilityText` on the first rich text element.

```
a!localVariables(
  local!currentPage: 4,
  local!pages: {
    "Home",
    "My Documents",
    "Strategy",
    "2018 Road Map"
  },
  a!richTextDisplayField(
    accessibilityText: "Breadcrumb",
    labelPosition: "COLLAPSED",
    value: {
      a!forEach(
        items: local!pages,
        expression: if(
          fv!isLast,
          a!richTextItem(text: fv!item, style: "STRONG"),
          {
            a!richTextItem(
              text: fv!item,
              link: a!dynamicLink(
                saveInto: {
                  a!save(
                    target: local!currentPage,
                    value: fv!index
                  ),
                  a!save(
                    local!pages,
                    rdrop(
                      local!pages,
                      length(local!pages) - fv!index
                    )
                  )
                }
              ),
              linkStyle: "STANDALONE"
            ),
            " ",
            a!richTextIcon(icon: "angle-right", color: "SECONDARY"),
            " "
          }
        )
      )
    }
  )
)
```

#### Cards with nested menu buttons
* Uses rich text link instead of card link to avoid nested controls (for menu button).
* Uses ariaLabel on `a!menuLayout` (instead of label) to provide correct programmatic label for control.
* Combines item name with “actions” for the aria label to avoid duplicate controls (e.g., multiple items called “actions”).

```
a!localVariables(
  local!fruits: { "Blueberries", "Kiwis", "Strawberries" },
  a!forEach(
    items: local!fruits,
    expression: a!cardLayout(
      contents: a!sideBySideLayout_internal(
        items: {
          a!sideBySideItem(
            item: a!richTextDisplayField(
              value: a!richTextItem(
                text: fv!item,
                link: a!dynamicLink(),
                linkStyle: "STANDALONE"
              )
            )
          ),
          a!sideBySideItem(
            item: a!menuLayout(
              label: null,
              icon: "ellipsis-v",
              style: "LINK",
              size: "SMALL",
              ariaLabel: fv!item & " Actions",
              options: {
                a!menuLayoutOption(primaryText: "Edit"),
                a!menuLayoutOption(primaryText: "Duplicate"),
                a!menuLayoutOption(primaryText: "Archive"),
                a!menuLayoutOption(primaryText: "Delete")
              },
              hasPulldown: false
            ),
            width: "MINIMIZE"
          )
        }
      ),
      marginBelow: "LESS"
    )
  )
)
```

#### Tabs using buttons
* Includes `accessibilityText` for selected tab.
* Applies to tabs using cards also.

```
a!localVariables(
  local!tabs: { "Summary", "News", "Related Actions" },
  local!selectedTab: 1,
  {
    a!buttonArrayLayout(
      buttons: a!forEach(
        items: local!tabs,
        expression: a!buttonWidget(
          label: fv!item,
          saveInto: if(
            local!selectedTab = fv!index,
            {},
            a!save(local!selectedTab, fv!index)
          ),
          width: "MINIMIZE",
          style: if(
            local!selectedTab = fv!index,
            "SOLID",
            "LINK"
          ),
          accessibilityText: if(
            local!selectedTab = fv!index,
            "Selected",
            {}
          )
        )
      )
    ),
    a!richTextDisplayField(
      labelPosition: "COLLAPSED",
      value: a!richTextItem(
        text: a!match(
          value: local!selectedTab,
          equals: 1,
          then: "The contents of the first tab would go here",
          equals: 2,
          then: "The contents of the second tab would go here",
          equals: 3,
          then: "The contents of the third tab would go here",
          default: "Something went wrong."
        ),
        style: "EMPHASIS"
      ),
      align: "CENTER"
    )
  }
)
```

[Return to top](#appian-sail-accessibility-checklist-and-examples)

[View on GitHub](https://github.com/pglevy/accessible-sail)