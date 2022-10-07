<script lang="ts">
	const TABLE_HEADER = ['ID', '価格', '有・無'];
	let Items = [
		{ id: 101, price: 300, isPrice: true },
		{ id: 202, price: 300, isPrice: false },
		{ id: 303, price: 300, isPrice: true }
	];

	const downLoadItemsAsCsv = () => {
		const bom = new Uint8Array([0xef, 0xbb, 0xbf]);
		const csv = Items.map((ring) => {
			return [ring.id, ring.price, ring.isPrice ? '有' : '無'].join(',');
		});
		csv.unshift(TABLE_HEADER.join(','));
		const blob = new Blob([bom, csv.join('\r\n')], { type: 'text/csv' });
		const url = URL.createObjectURL(blob);
		const a = document.createElement('a');
		a.href = url;
		a.download = 'Items.csv';
		a.click();
	};

	$: console.log(Items);
</script>

<table>
	<tr>
		{#each TABLE_HEADER as tableHeader}
			<th>{tableHeader}</th>
		{/each}
	</tr>
	<tbody>
		{#each Items as ring}
			<tr>
				<td>{ring.id}</td>
				<td><input type="number" bind:value={ring.price} /></td>
				<td>
					<input type="checkbox" bind:checked={ring.isPrice} />
					{ring.isPrice}</td>
			</tr>
		{/each}
	</tbody>
</table>

<button on:click={() => downLoadItemsAsCsv()}>CSVファイルとしてダウンロード</button>

<style>
	table {
		max-width: 900px;
		border-collapse: separate;
		border-spacing: 0;
		margin: 100px auto;
	}
	table td,
	th {
		text-align: center;
		border-left: 1px solid #a8b7c5;
		border-top: none;
		box-shadow: 5px -3px 5px 1px #eee inset;
		width: 200px;
		padding: 10px 0;
	}
	th {
		background-color: rgb(0, 0, 0);
		color: white;
	}

	table td:last-child {
		border-right: 1px solid #a8b7c5;
	}

	table tr:last-child td:first-child {
		border-radius: 0 0 0 5px;
	}

	table tr:last-child td:last-child {
		border-radius: 0 0 5px 0;
	}
	button {
		display: block;
		margin: 0 auto;
	}
</style>
