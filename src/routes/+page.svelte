<script lang="ts">
	const TABLE_HEADER = ['ID', '価格(有/無)'];
	let nowRings = [
		{ id: 101, isPrice: true },
		{ id: 202, isPrice: false },
		{ id: 303, isPrice: true }
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
		console.log(nowRings);
	};
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

<button on:click={() => downLoadnowRingsAsCsv()}>CSVファイルとして保存</button>

<style>
	table {
		max-width: 800px;
		border-collapse: separate;
		border-spacing: 0;
		margin: 100px auto;
	}
	table td,th {
		text-align: center;
		border-left: 1px solid #a8b7c5;
		border-top: none;
		box-shadow: 5px -3px 5px 1px #eee inset;
		width: 25%;
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
