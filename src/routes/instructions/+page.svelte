<script lang="ts">
	import * as Card from '$lib/components/ui/card/index.js';
</script>

<Card.Root>
	<Card.Header>
		<Card.Title>Import to Excel</Card.Title>
	</Card.Header>

	<Card.Content>
		<p>
			Once downloaded, open up Excel. Click Data then From Text/CSV to start the import process.
		</p>
		<img src="excel1.png" alt="" class="my-4 rounded-md" />

		<p>Use the headers.csv file you just downloaded</p>

		<img src="excel1b.png" alt="" class="my-4 rounded-md" />

		<p>Check Semicolon as the delimiter. Click Load</p>
		<img src="excel2.png" alt="" class="my-4 rounded-md" />

		<p>The dates are imported. Now you can copy and paste them to the schedule file</p>
		<img src="excel3.png" alt="" class="my-4 rounded-md" />
	</Card.Content>
</Card.Root>
