## SyntheticEvent

your event handlers will be passed instances of SyntheticEvent, a cross-browser wrapper around the browser's native event. it has the same interface as the browser's native event, including `stopPropagation()` and `preventDefault()`, except the events work identically across all browsers.

