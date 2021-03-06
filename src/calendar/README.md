# igxCalendar Component

The **igxCalendar** provides a way for the user to select date(s).  
A walkthrough of how to get started can be found [here](https://www.infragistics.com/products/ignite-ui-angular/angular/components/calendar.html)

## Dependencies
In order to be able to use **igxCalendar** you should keep in mind that it is dependent on **BrowserAnimationsModule**,
which must be imported **only once** in your application's AppModule, for example:
```typescript
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
@NgModule({
	imports: [
        BrowserAnimationsModule,
        ...
	]
})
export class AppModule {
}
```
Also the **igxCaledar** uses the [Intl](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/DateTimeFormat) WebAPI for localization and formatting of dates. Consider using the [appropriate polyfills](https://github.com/andyearnshaw/Intl.js/) if your target platform does not support them.


## Usage
Be sure to consult the API below for additional information.

### Importing the calendar in your application

```typescript
import { IgxCalendarComponent } from "igniteui-angular/main";
```
or
```typescript
import { IgxCalendarComponent } from "igniteui-angular/calendar";
```

Instantiate a calendar component in single selection mode displaying the current month.
```html
<igx-calendar></igx-calendar>
```


A range selection calendar with first day of week set to Monday and an event
handler when selection is done.
```html
<igx-calendar weekStart="1" selection="range" (onSelection)="eventHandler($event)"></igx-calendar>
```

A multiple selection calendar with different locale and templating for the subheader.
```html
<igx-calendar locale="ja-JP" selection="multi">
        <ng-template igxCalendarSubheader let-format>
                <span (click)="format.yearView()">{{ format.year.combined }}</span>
                <span (click)="format.monthView()">{{ format.month.combined | titlecase }}</span>
        </ng-template>
</igx-calendar>
```

The **igxCalendar** implements the `ControlValueAccessor` interface, providing two-way data-binding
and the expected behavior when used both in Template-driven or Reactive Forms.


### Keyboard navigation
When the **igxCalendar** component is focused:
- `PageUp` will move to the previous month.
- `PageDown` will move to the next month.
- `Shift + PageUp` will move to the previous year.
- `Shift + PageDown` will move to the next year.
- `Home` will focus the first day of the current month that is into view.
- `End` will focus the last day of the current month that is into view.

When a day inside the current month is focused:
- Arrow keys will navigate through the days.
- `Enter` will select the currently focused day.

## API Summary

### Inputs

- `vertical: boolean`

Controls the layout of the calendar component. When vertical is set to `true`
the calendar header will be rendered to the side of the calendar body.
Defaults to `false`.

- `weekStart: number | WEEKDAYS`

Controls the starting day of the weeek for the calendar.
Defaults to Sunday.

- `locale: string`

Controls the locale used for formatting and displaying the dates in the calendar.
The expected string should be a [BCP 47 language tag](http://tools.ietf.org/html/rfc5646).
The default value is `en-EN`.

- `selection: CalendarSelection | string`

Controls the type of selection in the calendar. Defaults to `CalendarSelection.SINGLE` which is equivalent to the string `single`.
Changing the selection type during 'runtime' will clear the previously selected values in the calendar.
The calendar header will not be rendered when the selection is either `multi` or `range`.

- `viewDate: Date`

Controls the year/month that will be presented in the default view when the calendar renders. By default it is the current year/month.

- `value: Date | Date[]`

Gets and sets the selected date(s) in the calendar component.
Both `multi` and `range` selection accepts single date values but they always return an array of date objects.

- `formatOptions: Object`

Controls the date-time components to use in formatted output, and their desired representations.
Consult [this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/DateTimeFormat)
for additional information on the available options.

The defaul values are listed below.
```typescript
{ day: 'numeric', month: 'short', weekday: 'short', year: 'numeric' }
```

- `formatViews: Object`

Controls whether the date parts in the different calendar views should be formatted according to the provided
`locale` and `formatOptions`.

The default values are listed below.
```typescript
{ day: false, month: true, year: false }
```

### Outputs

- `onSelection(): Date | Date[]`

Event fired when a value is selected through UI interaction.
Returns the selected value (depending on the type of selection).


### Methods

- `selectedDate(value: Date | Date[]): void`

Sets a new value for the calendar component. **Does not** trigger `onSelection` event.

### Templating

The **igxCalendar** supports templating of its header and subheader parts.
Just decorate a ng-template inside the calendar with `igxCalendarHeader` or `igxCalendarSubheader` directive
and use the context returned to customize the way the date is displayed.

The template decorated with the `igxCalendarHeader` directive is rendered only when the calendar selection is set to `single`.
The `igxCalendarSubheader` is available in all selection modes.

Example:

```html
<igx-calendar>
        <ng-template igxCalendarHeader let-parts>
                ...
        </ng-template>
        <ng-template igxCalendarSubheader let-parts>
        <!-- Let's change the default representation to YYYY-MM -->
                <span class="date__el" (click)="parts.monthView()">
                        {{ parts.month.combined }}
                </span>
                <span class="date__el" (click)="parts.yearView()">
                        {{ parts.year.combined }}
                </span>
        </ng-template>
</igx-calendar>
```
#### Template context

| Name      | Type     | Description                                                                  |
| :-------- | :------: | :--------------------------------------------------------------------------- |
| date      | Date     | The date object in the context of the template. See * below for details.       |
| full      | string   | The full date representation returned after applying the `formatOptions`.    |
| monthView | Function | A function which when called puts the calendar in month view.                |
| yearView  | Function | A function which when called puts the calendar in year view.                 |
| era       | Object   | The era date component (if applicable) formatted to the supplied locale.     |
| year      | Object   | The year date component (if applicable) formatted to the supplied locale.    |
| month     | Object   | The month date component (if applicable) formatted to the supplied locale.   |
| day       | Object   | The day date component (if applicable) formatted to the supplied locale.     |
| weekday   | Object   | The weekday date component (if applicable) formatted to the supplied locale. |

\* In the `igxCalendarHeader` context this is either the current date or the current selection of the calendar.
In the `igxCalendarSubheaderContext` this is the same as the `viewDate`

**NOTE:** All of the date components (year, month, etc.) are objects with the structure
```typescript
{
        value: string;
        literal: string;
        combined: string;
}
```
where `value` is the locale string representation of the date component, `literal` is the locale string separator (if any),
and `combined` is as the name suggests the combined output of the two.

**NOTE 2:** Mind that both in Internet Explorer and Edge all of the date parts will be empty strings as both browsers don't
implement the Intl API providing this functionality.
