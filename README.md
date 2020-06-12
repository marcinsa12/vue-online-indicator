# vue-online-indicator

## Installation

```
npm install vue-online-indicator
```

## Add it to your project

```
import onlineIndicator from 'vue-online-indicator'

Vue.use(onlineIndicator)
```

## Basic usage example
```
<template>
    <vue-online-indicator
        :timeout="2500"
        position="top-left"
        spanClass="online-indicator-text"
        class="online-indicator-wrapper"
        @online="doStuffOnline()"
        @offline="doStuffOffline()"

     />
</template>
```

## Props

| Name            | Type   | Required? | Default              | Description                                                 |
| --------------  | ------ | --------- | ---------            | ----------------------------------------------------------- |
| `position`      | String | No        | 'top-right'          | 'top-right/left/center', 'bottom-right/left/center/         |
| `timeout`       | Number | No        | '3500'               | time to dismiss notification. Set to 0 to keep it.          |
| `styles`        | Object | No        | '{}'                 | Styling the `div` wrapping element.                         |
| `spanStyles`    | Object | No        | '{}'                 | Styling the `span` element inside notification bar .        |
| `class`         | String | No        | ''                   | Class name for `div` wrapping element .                     |
| `spanClass`     | String | No        | ''                   | Class name for `span` element inside notification bar.      |