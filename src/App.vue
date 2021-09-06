<template>
  <div id="main">
    <div class="container my-5">
      <h1 class="title text-center">Weather in</h1>
      <form class="search-location">
        <input
          type="text"
          class="form-control text-muted form-rounded p-4 shadow-sm"
          placeholder="What City?"
          autocomplete="off"
        />
      </form>
      <div class="card rounded my-3 shadow-lg back-card overflow-hidden">
        <!-- Top of card starts here -->
        <div class="card-top text-center" style="margin-bottom: 15rem">
          <div class="city-name my-3">
            <p>İstanbul</p>
            <span>...</span>
            <p class="">NG</p>
          </div>
        </div>
        <!-- top of card ends here -->

        <!--card middle body, card body used cos I want to just update the innerHTML -->
        <div class="card-body">
          <!-- card middle starts here -->
          <div class="card-mid">
            <div class="row">
              <div class="col-12 text-center temp">
                <span>20&deg;C</span>
                <p class="my-4">Bulutlar heryerde</p>
              </div>
            </div>
            <div class="row">
              <div class="col d-flex justify-content-between px-5 mx-5">
                <p>
                  <img src="" alt="" />
                  18&deg;C
                </p>
                <p>
                  <img src="" alt="" />
                  25}&deg;C
                </p>
              </div>
            </div>
          </div>
          <!-- card middle ends here -->

          <!-- card bottom starts here -->
          <div class="card-bottom px-5 py-4 row">
            <div class="col text-center">
              <p>20&deg;C</p>
              <span>Feels like</span>
            </div>
            <div class="col text-center">
              <p>55%</p>
              <span>humidity</span>
            </div>
          </div>

          <!-- card bottom ends here -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      weather: {
        cityName: "İstanbul",
        country: "TR",
        temperature: 12,
        description: "Bulutlar her yerde",
        lowTemp: "19",
        highTemp: "30",
        feelsLike: "20",
        humidity: "55",
      },
    };
  },
};
</script>

<style>
@import "./assets/custom.css";
</style>
