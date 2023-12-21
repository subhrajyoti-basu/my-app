<script lang="ts">
  import { Calendar as CalendarIcon } from "lucide-svelte";
  import type { DateRange, Selected } from "bits-ui";
  import {
    CalendarDate,
    DateFormatter,
    getLocalTimeZone,
    type DateValue,
  } from "@internationalized/date";
  import { cn } from "$lib/utils";
  import { Button } from "$lib/components/ui/button";
  import { RangeCalendar } from "$lib/components/ui/range-calendar";

  import * as Dialog from "./ui/dialog";
  import * as Select from "./ui/select";

  const df = new DateFormatter("en-US", {
    dateStyle: "medium",
  });

  const today = new Date();

  type Time = {
    hour: Selected<unknown>;
    minutes: Selected<unknown>;
    time: Selected<unknown>;
  };

  let startTime: Time = {
    hour: { value: "00", label: "00" },
    minutes: { value: "00", label: "00" },
    time: { value: "AM", label: "AM" },
  };
  let endTime: Time = {
    hour: { value: "00", label: "00" },
    minutes: { value: "00", label: "00" },
    time: { value: "AM", label: "AM" },
  };

  let year: Selected<unknown> = {
    value: today.getFullYear(),
    label: today.getFullYear().toString(),
  };

  let value: DateRange | undefined = {
    start: new CalendarDate(
      year.value as number,
      today.getMonth() + 1,
      today.getDate() + 1
    ),
    end: new CalendarDate(
      year.value as number,
      today.getMonth() + 1,
      today.getDate() + 1
    ).add({ days: 20 }),
  };

  let startValue: DateValue | undefined = undefined;
  
  function generateNumbers(num: number) {
    let numbers = [];
    for (let i = 0; i <= num; i++) {
      let formattedNumber = ("0" + i).slice(-2);
      numbers.push(formattedNumber);
    }
    return numbers;
  }

  function generateYears(currentYear:number, yearRange:number){
    const yearArray = []

    for(let i = - yearRange; i <= yearRange; i++){
      yearArray.push(currentYear + i)
    }

    return yearArray
  }

  const generatedHour = generateNumbers(12);
  const generatedMinute = generateNumbers(59);
  const generatedYears = generateYears(today.getFullYear(), 5)

  type date = {
    day: number | undefined;
    month: number | undefined;
    year: number | undefined;
  };

  let modalClose: boolean = true;

  

  function handleOnclick() {
    const start: date = {
      day: value?.start?.day,
      month: value?.start?.month,
      year: value?.start?.year,
    };
    const end: date = {
      day: value?.end?.day,
      month: value?.end?.month,
      year: value?.end?.year,
    };
    console.log({ start, end }, startTime, endTime);
    modalClose = false;
  }
</script>

<div class="grid gap-2">
  <Dialog.Root bind:open={modalClose}>
    <Dialog.Trigger asChild let:builder>
      <Button
        variant="outline"
        class={cn(
          "w-[300px] justify-start text-left font-normal",
          !value && "text-muted-foreground"
        )}
        builders={[builder]}
      >
        <CalendarIcon class="mr-2 h-4 w-4" />
        {#if value && value.start}
          {#if value.end}
            {df.format(value.start.toDate(getLocalTimeZone()))} - {df.format(
              value.end.toDate(getLocalTimeZone())
            )}
          {:else}
            {df.format(value.start.toDate(getLocalTimeZone()))}
          {/if}
        {:else if startValue}
          {df.format(startValue.toDate(getLocalTimeZone()))}
        {:else}
          Pick a date
        {/if}
      </Button>
    </Dialog.Trigger>
    <Dialog.Content class="max-w-fit bg-[#18181a] text-neutral-400">
      <div class="mt-8">
        <Select.Root bind:selected={year} onSelectedChange={(a) => {
          value?.start?.set({year: 2020})
          
        }}>
          <Select.Trigger>
            <Select.Value placeholder="Select Year" />
          </Select.Trigger>
          <Select.Content>
            {#each generatedYears as number}
              <Select.Item value={number}>{number}</Select.Item>
            {/each}
          </Select.Content>
        </Select.Root>
      </div>
      <RangeCalendar
        bind:value={value}
        bind:startValue
        initialFocus
        numberOfMonths={2}
        placeholder={value?.start}
      />
      <div class="flex justify-between">
        <div class="flex space-x-3">
          <Select.Root bind:selected={startTime.hour}>
            <Select.Trigger class="w-[50px]">
              <Select.Value placeholder="00" />
            </Select.Trigger>
            <Select.Content>
              {#each generatedHour as number}
                <Select.Item class="px-1" value={number}>{number}</Select.Item>
              {/each}
            </Select.Content>
          </Select.Root>
          <Select.Root bind:selected={startTime.minutes}>
            <Select.Trigger class="w-[50px]">
              <Select.Value placeholder="00" />
            </Select.Trigger>
            <Select.Content>
              {#each generatedMinute as number}
                <Select.Item class="px-1" value={number}>{number}</Select.Item>
              {/each}
            </Select.Content>
          </Select.Root>
          <Select.Root bind:selected={startTime.time}>
            <Select.Trigger class="w-[80px]">
              <Select.Value placeholder="AM" />
            </Select.Trigger>
            <Select.Content>
              <Select.Item class="px-1" value="AM">AM</Select.Item>
              <Select.Item class="px-1" value="PM">PM</Select.Item>
            </Select.Content>
          </Select.Root>
        </div>
        <div class="flex space-x-3">
          <Select.Root bind:selected={endTime.hour}>
            <Select.Trigger class="w-[50px]">
              <Select.Value placeholder="00" />
            </Select.Trigger>
            <Select.Content>
              {#each generatedHour as number}
                <Select.Item class="px-1" value={number}>{number}</Select.Item>
              {/each}
            </Select.Content>
          </Select.Root>
          <Select.Root bind:selected={endTime.minutes}>
            <Select.Trigger class="w-[50px]">
              <Select.Value placeholder="00" />
            </Select.Trigger>
            <Select.Content>
              {#each generatedMinute as number}
                <Select.Item class="px-1" value={number}>{number}</Select.Item>
              {/each}
            </Select.Content>
          </Select.Root>
          <Select.Root bind:selected={endTime.time}>
            <Select.Trigger class="w-[80px]">
              <Select.Value placeholder="AM" />
            </Select.Trigger>
            <Select.Content>
              <Select.Item class="px-1" value="AM">AM</Select.Item>
              <Select.Item class="px-1" value="PM">PM</Select.Item>
            </Select.Content>
          </Select.Root>
        </div>
      </div>
      <div class="flex justify-between items-center">
        <div class="text-sm">
          {#if value && value.start}
            {#if value.end}
              {df.format(value.start.toDate(getLocalTimeZone()))} - {df.format(
                value.end.toDate(getLocalTimeZone())
              )}
            {:else}
              {df.format(value.start.toDate(getLocalTimeZone()))}
            {/if}
          {:else if startValue}
            {df.format(startValue.toDate(getLocalTimeZone()))}
          {:else}
            Pick a date
          {/if}
        </div>
        <div class="text-sm flex space-x-2 items-center">
          <div>
            {startTime.hour.value +
              ":" +
              startTime.minutes.value +
              ":" +
              startTime.time.value}
          </div>
          <div>
            {endTime.hour.value +
              ":" +
              endTime.minutes.value +
              ":" +
              endTime.time.value}
          </div>
          <div>
            <Button
              class="bg-neutral-700 h-8"
              on:click={() => (modalClose = false)}>Cancel</Button
            >
            <Button class="bg-indigo-600 h-8" on:click={handleOnclick}
              >Apply</Button
            >
          </div>
        </div>
      </div>
    </Dialog.Content>
  </Dialog.Root>
</div>
