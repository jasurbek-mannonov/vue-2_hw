<script>
export default {
  data() {
    return {
      cars: [],
      car: {},
      selectedCarBrand: "",
      brandList: ["Chevrolet", "Lada", "Tesla", "BMW", "Mercedes"],
      carModel: "",
      leaveTime: "",
      parkingLotNumber: null,
      parkingLotCapacity: 20,
    };
  },
  computed: {
    freeSpaces() {
      return this.parkingLotCapacity - this.cars.length;
    },
    occupiedSpaces() {
      return this.cars.length;
    },
    carCounts() {
      const counts = {};
      for (const car of this.cars) {
        if (counts[car.brand]) {
          counts[car.brand]++;
        } else {
          counts[car.brand] = 1;
        }
      }
      return counts;
    },
    carsEnteredToday() {
      const today = new Date().setHours(0, 0, 0, 0);
      return this.cars.filter(
        (car) => new Date(car.entryTime).setHours(0, 0, 0, 0) === today
      ).length;
    },
    carsToBeCountedToday() {
      const today = new Date().setHours(0, 0, 0, 0);
      return this.cars.filter(
        (car) => new Date(car.leaveTime).setHours(0, 0, 0, 0) === today
      ).length;
    },
  },
  methods: {
    registerCar() {
      if (
        this.selectedCarBrand &&
        this.carModel &&
        this.leaveTime &&
        this.parkingLotNumber !== null
      ) {
        const car = {
          brand: this.selectedCarBrand,
          model: this.carModel,
          entryTime: new Date(),
          leaveTime: this.leaveTime,
          parkingLot: this.parkingLotNumber,
        };
        if (this.cars.length >= this.parkingLotCapacity) {
          alert("Bo'sh joy mavjud emas!");
        } else {
          this.cars.push(car);
          this.clearForm();
        }
      } else {
        alert("Barchasini to'ldiring");
      }
    },
    clearForm() {
      this.selectedCarBrand = "";
      this.carModel = "";
      this.leaveTime = "";
      this.parkingLotNumber = null;
    },
  },
};
</script>

<template>
  <div class="container p-4">
    <h1 class="text-center mb-2">Mashinalar Turargohi</h1>
    <h2 class="text-center mb-2">Mashinani ro'yxatdan o'tkazish</h2>
    <div>
      <label for="carBrand">Mashina brendi:</label>
      <select
        class="form-control mb-4"
        id="carBrand"
        v-model="selectedCarBrand"
      >
        <option v-for="(brand, index) of brandList" :key="index" :value="brand">{{ brand }}</option>
      </select>
    </div>
    <div>
      <label for="carModel">Mashina rusumi:</label>
      <input
        class="form-control mb-4"
        id="carModel"
        v-model="carModel"
        type="text"
      />
    </div>
    <div>
      <label for="leaveTime">Chiqish vaqti:</label>
      <input
        class="form-control mb-4"
        id="leaveTime"
        v-model="leaveTime"
        type="datetime-local"
      />
    </div>
    <div>
      <label for="parkingLot">Joy raqami:</label>
      <input
        class="form-control mb-4"
        id="parkingLot"
        v-model="parkingLotNumber"
        type="number"
      />
    </div>

    <div class="d-flex justify-content-center">
      <button class="btn btn-success mb-4" @click="registerCar">
        Ro'yxatdan o'tkazish
      </button>
    </div>
    <h1 class="text-center mb-4">Turargohdagi mashinalar haqida ma'lumot</h1>
    <table class="table table-bordered" width="100%">
      <thead>
        <tr>
          <th>j/r</th>
          <th>Rusumi</th>
          <th>Brendi</th>
          <th>Kirgan vaqt</th>
          <th>Chiqadigan vaqt</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(car, index) of cars" :key="index">
          <td>{{ car.parkingLot }}</td>
          <td>{{ car.model }}</td>
          <td>{{ car.brand }}</td>
          <td>{{ car.entryTime }}</td>
          <td>{{ car.leaveTime }}</td>
        </tr>
      </tbody>
    </table>
    <h1 class="text-center mb-4">Turargoh statistikasi</h1>
    <p>Bo'sh o'rinlar: {{ freeSpaces }}</p>
    <p>Band o'rinlar miqdori: {{ occupiedSpaces }}</p>
    <p>Mashinalar rusumi miqdori: </p>
    <ul>
      <li v-for="[key, value] of Object.entries(carCounts)" :key="key">
        {{ key }}: {{ value }} ta
      </li>
    </ul>
    <p>Bugun kirgan mashinalar: {{ carsEnteredToday }}</p>
    <p>Bugun chiqishi kerak bo'lgan mashinalar: {{ carsToBeCountedToday }}</p>
  </div>
</template>

<style></style>
