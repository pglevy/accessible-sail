---
title: Home
---

# Appian SAIL Accessibility Checklist and Examples

How to use this reference:
* Review the checklist when creating SAIL interfaces.
* Use examples as starting points for incorporating best practices.
* Ask questions if you come across something you’re not sure about.

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