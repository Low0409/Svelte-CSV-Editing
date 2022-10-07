<script lang="ts">

	const TABLE_HEADER = ['ID', '価格','有・無'];
	let nowRings = [
		{ id: 101, price: 300,isPrice: true },
		{ id: 202, price: 300,isPrice: false },
		{ id: 303, price: 300,isPrice: true }
	];

	const updatenowRingsisPrice = (id: number, isPrice: boolean) => {
		nowRings = nowRings.map((ring) => {
			if (ring.id === id) {
				ring.isPrice = isPrice;
			}
			return ring;
		});
	};

	const downLoadnowRingsAsCsv = () => {
		const bom = new Uint8Array([0xef, 0xbb, 0xbf]);
		const csv = nowRings.map((ring) => {
			return [ring.id, ring.price, ring.isPrice ? '有' : '無'].join(',');
		});
		csv.unshift(TABLE_HEADER.join(','));
		const blob = new Blob([bom, csv.join('\r\n')], { type: 'text/csv' });
		const url = URL.createObjectURL(blob);
		const a = document.createElement('a');
		a.href = url;
		a.download = 'nowRings.csv';
		a.click();
	};

	$:console.log(nowRings);

</script>

<table>
	<tr>
		{#each TABLE_HEADER as tableHeader}
			<th>{tableHeader}</th>
		{/each}
	</tr>
	<tbody>
		{#each nowRings as ring}
			<tr>
				<td>{ring.id}</td>
				<td><input type="number" bind:value={ring.price} /></td>
				<td>
					<input
						type="checkbox"
						bind:checked={ring.isPrice}
						on:change={() => updatenowRingsisPrice(ring.id, ring.isPrice)}
					/>
					{ring.isPrice}</td
				>
			</tr>
		{/each}
	</tbody>
</table>

<button on:click={() => downLoadnowRingsAsCsv()}>CSVファイルとしてダウンロード</button>

<style>
	table {
		max-width: 900px;
		border-collapse: separate;
		border-spacing: 0;
		margin: 100px auto;
	}
	table td,th {
		text-align: center;
		border-left: 1px solid #a8b7c5;
		border-top: none;
		box-shadow: 5px -3px 5px 1px #eee inset;
		width: 200px;
		padding: 10px 0;
	}
    th{
        background-color: rgb(150, 255, 248);
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
</style>
