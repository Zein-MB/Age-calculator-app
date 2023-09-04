<script>
export default {
  data() {
    return {
      years: null,
      nowYears: new Date().getFullYear(),
      newYears: '--',
      months: null,
      nowMonths: new Date().getMonth() + 1,
      newMonths: '--',
      days: null,
      nowDays: new Date().getDate(),
      newDays: '--',
      daysPattern: /^(0[0-9]|1\d|2\d|3[01])$/,
      matchDays: true,
      monthsPattern: /^(0[1-9]|1[0-2])$/,
      matchMonths: true,
      yearsPattern: /^(19[3-9][0-9]|200[0-9]|201[0-9])$/,
      matchYears: true,
    };
  },
  methods: {
    // If there is any error while submitting don't submit, show errors
    checkErrors: function (e) {
      if (this.years == '' || this.years == null ||
        this.months == '' || this.months == null ||
        this.days == '' || this.days == null) {
        e.preventDefault()
      }
    },
    checkValue(e) {
      if (!this.daysPattern.test(this.days)) {
        this.matchDays = !this.matchDays
      }
      else if (!this.monthsPattern.test(this.months)) {
        this.matchMonths = !this.matchMonths
      }
      else if (!this.yearsPattern.test(this.years)) {
        this.matchYears = !this.matchYears
      } else {
        if (this.months == this.nowMonths) {
          if (this.days > this.nowDays) {
            this.newYears = this.nowYears - this.years - 1;
            this.newMonths = 12 - (this.months - this.nowMonths);
            if (
              this.months == 4 ||
              this.months == 6 ||
              this.months == 9 ||
              this.months == 11
            ) {
              this.newDays = 30 - (this.days - this.nowDays)
            } else if (this.months == 2) {
              this.newDays = 28 - (this.days - this.nowDays)
            } else {
              this.newDays = 31 - (this.days - this.nowDays)
            }
          }
          if (this.days < this.nowDays) {
            this.newYears = this.nowYears - this.years;
            this.newMonths = this.months - this.nowMonths;
            this.newDays = this.nowDays - this.days;
          }
          if (this.days == this.nowDays) {
            this.newYears = this.nowYears - this.years;
            this.newMonths = this.nowMonths - this.months;
            this.newDays = this.nowDays - this.days;
          }
        } else if (this.months > this.nowMonths) {
          this.newYears = this.nowYears - this.years - 1;
          if (this.days < this.nowDays) {
            this.newMonths = 12 - (this.months - this.nowMonths);
            if (
              this.months == 4 ||
              this.months == 6 ||
              this.months == 9 ||
              this.months == 11
            ) {
              this.newDays = 30 - (this.nowDays - this.days);
            } else if (this.months == 2) {
              this.newDays = 28 - (this.nowDays - this.days);
            } else {
              this.newDays = 31 - (this.nowDays - this.days)
            }
          }
          if (this.days == this.nowDays) {
            this.newMonths = 12 - (this.months - this.nowMonths);
            this.newDays = this.nowDays - this.days;
          }
          if (this.days > this.nowDays) {
            this.newMonths = 12 - (this.months - this.nowMonths);
            if (
              this.months == 4 ||
              this.months == 6 ||
              this.months == 9 ||
              this.months == 11
            ) {
              this.newDays = 30 - (this.days - this.nowDays);
            } else if (this.months == 2) {
              this.newDays = 28 - (this.days - this.nowDays);
            } else {
              this.newDays = 31 - (this.days - this.nowDays);
            }
          }
        } else if (this.months < this.nowMonths) {
          this.newYears = this.nowYears - this.years;
          if (this.days < this.nowDays) {
            this.newMonths = this.nowMonths - this.months;
            this.newDays = this.nowDays - this.days;
          }
          if (this.days == this.nowDays) {
            this.newMonths = this.nowMonths - this.months;
            this.newDays = this.days - this.nowDays;
          }
          if (this.days > this.nowDays) {
            this.newMonths = (this.nowMonths - this.months) - 1;
            if (
              this.months == 4 ||
              this.months == 6 ||
              this.months == 9 ||
              this.months == 11
            ) {
              this.newDays = 30 - (this.days - this.nowDays);
            } else if (this.months == 2) {
              this.newDays = 28 - (this.days - this.nowDays);
            } else {
              this.newDays = 31 - (this.days - this.nowDays);
            }
          }
        }
      }
    },

    resetInputs() {
      this.days = null;
      this.months = null;
      this.years = null;
    }
  }
}
</script>

<template>
  <form action="POST" @submit.prevent="checkValue">
    <div class="inputs">
      <div>
        <label for="Day" :class="days == '' || !matchDays ? 'empty-label' : ''">Day</label>
        <input type="number" placeholder="DD" id="Day" v-model="days" :class="days == '' ||
          !matchDays ? 'empty' : ''" @keyup="days > 0 && days < 10 ? days = `0${days}` : days = `${days}`" />
        <p class="empty-error" v-show="days == ''">This field is required</p>
        <p class="invalid-error" v-show="!matchDays">Must be a valid day</p>
      </div>
      <div>
        <label for="Month" :class="months == '' || !matchMonths ? 'empty-label' : ''">Month</label>
        <input type="number" placeholder="MM" id="Month" v-model="months"
          :class="months == '' || !matchMonths ? 'empty' : ''"
          @keyup="months > 0 && months < 10 ? months = `0${months}` : months = `${months}`" />
        <p class="empty-error" v-show="months == ''">This field is required</p>
        <p class="invalid-error" v-show="!matchMonths">Must be a valid month</p>
      </div>
      <div>
        <label for="Year" :class="years == '' || !matchYears ? 'empty-label' : ''">Year</label>
        <input type="number" placeholder="YYYY" id="Year" v-model="years"
          :class="years == '' || !matchYears ? 'empty' : ''" />
        <p class="empty-error" v-show="years == ''">This field is required</p>
        <p class="invalid-error" v-show="!matchYears">Must be a valid year</p>
      </div>
      <abbr title="Reset Inputs">
        <Transition>
          <button class="reset-btn" type="button" v-show="days != null || months != null || years != null" @click="resetInputs">
            <img src="../assets/images/reset-btn.svg" alt="">
          </button>
        </Transition>
      </abbr>
    </div>
    <div class="submit">
      <button class="submit-btn" type="submit" @click="checkErrors" aria-label="submit">
        <img src="../assets/images/icon-arrow.svg" alt="" />
      </button>
    </div>
  </form>
  <div class="result">
    <p><span>{{ this.newYears }}</span> <b>years</b></p>
    <p><span>{{ this.newMonths }}</span> <b>months</b></p>
    <p><span>{{ this.newDays }}</span> <b>days</b></p>
  </div>
</template>
