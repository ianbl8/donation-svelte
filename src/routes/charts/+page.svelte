<script>
    // @ts-nocheck
    import Header from "$lib/Header.svelte";
    import MainNavigator from "$lib/MainNavigator.svelte";
    import Chart from "svelte-frappe-charts";
    import { onMount } from "svelte";
    import { donationService } from "../../services/donation-service";

    let data = {
        labels: ["Paypal", "Direct"],
        datasets: [
            {
                values: [0, 0]
            }
        ]
    };

    onMount(async () => {
        let donationList = await donationService.getDonations();
        donationList.forEach((donation) => {
            if (donation.method == "Paypal") {
                data.datasets[0].values[0] += donation.amount;
            } else if (donation.method == "Direct") {
                data.datasets[0].values[1] += donation.amount;
            }
        });
    });
</script>

<Header>
    <MainNavigator />
</Header>

<div class="columns">
    <div class="column has-text-centered">
      <h1 class="title is-4">Donations: Bar Chart</h1>
      <Chart data={data} type="bar"/>
    </div>
    <div class="column has-text-centered">
      <h1 class="title is-4">Donations: Pie Chart</h1>
      <Chart data={data} type="pie"/>
    </div>
  </div>