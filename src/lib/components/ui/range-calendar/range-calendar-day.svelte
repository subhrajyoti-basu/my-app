<script lang="ts">
	import { RangeCalendar as RangeCalendarPrimitive } from "bits-ui";
	import { buttonVariants } from "$lib/components/ui/button";
	import { cn } from "$lib/utils";

	type $$Props = RangeCalendarPrimitive.DayProps;
	type $$Events = RangeCalendarPrimitive.DayEvents;

	export let date: $$Props["date"];
	export let month: $$Props["month"];
	let className: $$Props["class"] = undefined;
	export { className as class };
</script>

<RangeCalendarPrimitive.Day
	on:click
	{date}
	{month}
	class={cn(
		buttonVariants({ variant: "ghost" }),
		"h-9 w-9 p-0 font-normal data-[selected]:opacity-100",
		"[&[data-today]:not([data-selected])]:bg-accent [&[data-today]:not([data-selected])]:text-blue-800",
		// Selection Start
		"data-[selection-start]:bg-[#65a30d] data-[selection-start]:text-black data-[selection-start]:hover:bg-primary data-[selection-start]:hover:text-primary-foreground data-[selection-start]:focus:bg-[#65a30d] data-[selection-start]:focus:text-primary-foreground",
		// Selection End
		"data-[selection-end]:bg-foreground data-[selection-end]:border-2 data-[selection-end]:border-blue-800 data-[selection-end]:text-primary-foreground data-[selection-end]:hover:bg-primary data-[selection-end]:hover:text-primary-foreground data-[selection-end]:focus:bg-[#65a30d] data-[selection-end]:focus:text-primary-foreground",
		// Outside months
		"data-[outside-month]:text-white data-[outside-month] [&[data-outside-month][data-selected]] [&[data-outside-month][data-selected]]:text-white [&[data-outside-month][data-selected]] data-[outside-month]:pointer-events-none",
		// Disabled
		"data-[disabled]:text-muted-foreground data-[disabled]:opacity-50",
		// Unavailable
		"data-[unavailable]:line-through data-[unavailable]:text-destructive-foreground",
		className
	)}
	{...$$restProps}
	let:disabled
	let:unavailable
	let:builder
>
	<slot {disabled} {unavailable} {builder}>
		{date.day}
	</slot>
</RangeCalendarPrimitive.Day>
