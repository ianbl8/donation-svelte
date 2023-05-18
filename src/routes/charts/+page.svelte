<script>
	// @ts-nocheck
	import Header from '$lib/Header.svelte';
	import MainNavigator from '$lib/MainNavigator.svelte';
	import Chart from 'svelte-frappe-charts';
	import { onMount } from 'svelte';
	import { donationService } from '../../services/donation-service';

	let totalByMethod = {
		labels: ['Paypal', 'Direct'],
		datasets: [
			{
				values: [0, 0]
			}
		]
	};

	let totalByCandidate = {
		labels: [],
		datasets: [
			{
				values: [0, 0]
			}
		]
	};

	function populateByMethod(donationList) {
		donationList.forEach((donation) => {
			if (donation.method == 'Paypal') {
				totalByMethod.datasets[0].values[0] += donation.amount;
			} else if (donation.method == 'Direct') {
				totalByMethod.datasets[0].values[1] += donation.amount;
			}
		});
	}

	function populateByCandidate(donationList, candidates) {
		totalByCandidate.labels = [];
		candidates.forEach((candidate) => {
			totalByCandidate.labels.push(`${candidate.lastName}, ${candidate.firstName}`);
			totalByCandidate.datasets[0].values.push(0);
		});
		candidates.forEach((candidate, i) => {
			donationList.forEach((donation) => {
				if (donation.candidate._id == candidate._id) {
					totalByCandidate.datasets[0].values[i] += donation.amount;
				}
			});
		});
	}

	onMount(async () => {
		let donationList = await donationService.getDonations();
		let candidates = await donationService.getCandidates();
		populateByMethod(donationList);
		populateByCandidate(donationList, candidates);
	});
</script>

<Header>
	<MainNavigator />
</Header>

<div class="columns">
	<div class="column has-text-centered">
		<h1 class="title is-4">Donations to date</h1>
		<Chart data={totalByMethod} type="pie" />
	</div>
	<div class="column has-text-centered">
		<h1 class="title is-4">Donations by candidate</h1>
		<Chart data={totalByCandidate} type="bar" />
	</div>
</div>
