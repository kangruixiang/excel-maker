<script lang="ts">
	import dayjs, { type Dayjs } from 'dayjs';
	import { Label } from '$lib/components/ui/label';
	import { Calendar } from '$lib/components/ui/calendar';
	import { Input } from '$lib/components/ui/input';
	import * as Card from '$lib/components/ui/card/index.js';
	import { getLocalTimeZone, type DateValue } from '@internationalized/date';
	import { Button } from '$lib/components/ui/button';

	let inputDate = $state<DateValue | undefined>();
	let numberOfWeeks = $state<number>(52);

	function makeDateHeader(inputDate: DateValue | undefined, numberofWeeks: number) {
		if (inputDate === undefined) {
			return;
		}

		const startDate = dayjs(inputDate.toDate(getLocalTimeZone()));

		function getOneWeekFormat(inputDate: Dayjs, multiplyer: number) {
			const weekdaysStart = startDate.add(multiplyer * 7, 'day');
			const weekdaysEnd = weekdaysStart.add(4, 'day').format('MM/DD');
			const weekendStart = weekdaysStart.add(5, 'day').format('MM/DD');
			const weekendEnd = weekdaysStart.add(6, 'day').format('MM/DD');
			const weekdays = `${weekdaysStart.format('MM/DD')}-${weekdaysEnd}`;
			const weekend = `${weekendStart}, ${weekendEnd}`;

			const week = [weekdays, weekend];
			return week;
		}

		let arrayofHeader: string[] = [];
		const weeks = new Array(numberofWeeks).fill(undefined).map((value, index) => {
			const result = getOneWeekFormat(startDate, index);
			arrayofHeader = [...arrayofHeader, ...result];
		});

		const stringOfHeaders = arrayofHeader.join(';');

		return stringOfHeaders;
	}

	function downloadCSV(csvString: string | undefined) {
		if (csvString === undefined) {
			return;
		}

		// Create a Blob with the CSV data
		const blob = new Blob([csvString], { type: 'text/csv' });

		// Create a temporary URL for the Blob
		const url = URL.createObjectURL(blob);

		// Create a temporary link element
		const link = document.createElement('a');
		link.href = url;
		link.download = 'headers.csv';

		// Append the link to the body
		document.body.appendChild(link);

		// Trigger the click event to download the file
		link.click();

		// Clean up: remove the link and revoke the URL object
		document.body.removeChild(link);
		URL.revokeObjectURL(url);
	}
</script>

<div class="mx-auto my-4 max-w-2xl rounded-lg bg-white p-4">
	<Card.Root>
		<Card.Header>
			<Card.Title>Excel Headers</Card.Title>
		</Card.Header>
		<Card.Content>
			<p>
				This will generate the header dates for a specified number of weeks including the weekends.
				An example is seen below:
			</p>

			<img src="header.png" alt="" class="my-4 rounded-md" />
		</Card.Content>
		<Card.Content class="space-around flex gap-4">
			<p class="">Start by picking the first date from the right.</p>

			<Calendar type="single" bind:value={inputDate} />
		</Card.Content>
		<Card.Content>
			<div>
				<p>
					Next, pick the number of weeks you want to generate headers. Default for a year is 52:
				</p>
				<Input type="number" bind:value={numberOfWeeks} class="my-4" />
			</div>
		</Card.Content>
		<Card.Content class="flex justify-end">
			<Button onclick={() => downloadCSV(makeDateHeader(inputDate, numberOfWeeks))}>Submit</Button>
		</Card.Content>
	</Card.Root>
</div>
