<script>
  // @ts-nocheck
  import Chart from "svelte-frappe-charts";
  import { onMount } from "svelte";
  import { donationService } from "../services/donation-service";
  import { latestDonation } from "../stores";

  let totalByMethod = {
      labels: ["Paypal", "Direct"],
      datasets: [
          {
              values: [0, 0]
          }
      ]
  };

  function populateByMethod(donationList) {
      donationList.forEach((donation) => {
          if (donation.method == "Paypal") {
              totalByMethod.datasets[0].values[0] += donation.amount;
          } else if (donation.method == "Direct") {
              totalByMethod.datasets[0].values[1] += donation.amount;
          }
      });
  }

  async function refreshChart() {
      let donationList = await donationService.getDonations();
      populateByMethod(donationList);
  }

  onMount(async () => {
      refreshChart();
  });

  latestDonation.subscribe(async (donation) => {
        if (donation) {
            await refreshChart();
        }
    });
</script>

<h1 class="title is-4">By Method</h1>
<Chart data={totalByMethod} type="pie" />