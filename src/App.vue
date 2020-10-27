<template>
  <div>
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">
      <b-form-group id="input-group-1" label="Appointment ID:" label-for="input-1">
        <b-form-input
          id="input-1"
          v-model="form.id"
          required
          placeholder="Enter the Appointment number"
        ></b-form-input>
      </b-form-group>
      <b-form-group
        id="input-group-2"
        label="Email address:"
        label-for="input-2"
        description="We'll never share your email with anyone else."
      >
        <b-form-input
          id="input-2"
          v-model="form.email"
          type="email"
          required
          placeholder="Enter email"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-3" label="Your Name:" label-for="input-3">
        <b-form-input
          id="input-3"
          v-model="form.name"
          required
          placeholder="Enter name"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-4" label="Appointment Time:" label-for="input-4">
     <div>
  
   
      <b-container >
  <b-row>
    <b-col sm="*"> <b-form-datepicker :date-disabled-fn="dateDisabled"  required id="example-datepicker" v-model="form.date" class="mb-16"></b-form-datepicker></b-col>
    <b-col sm="*"> <b-form-timepicker  v-model="form.time" show-seconds locale="en" class="mb-16"></b-form-timepicker></b-col>
  </b-row>
</b-container>
       
         
   
 
 
   
  </div>
      </b-form-group>
      <b-button type="submit" variant="primary">Submit</b-button>
      <b-button type="reset" variant="danger">Reset</b-button>
    </b-form>
    <b-card class="mt-3" header="Form Data Result">
      <pre class="m-0">{{ form }}</pre>
    </b-card>
  </div>
</template>

<script>
  export default {
    data() {

const now = new Date()
      const today = new Date(now.getFullYear(), now.getMonth(), now.getDate())
      // 15th two months prior
      const minDate = new Date(today)
      minDate.setMonth(minDate.getMonth())
      minDate.setDate(1)
      // 15th in two monhs
      const maxDate = new Date(today)
      maxDate.setMonth(maxDate.getMonth() + 2)
      maxDate.setDate(15)



      return {
        item:[],
        form: {
          id: "",
          email: '',
          name: '',
          date: '',
          time:"",
          min:minDate,
        },
        foods: [{ text: 'Select One', value: null }, 'Carrots', 'Beans', 'Tomatoes', 'Corn'],
        show: true
      }
    },
    methods: {
          dateDisabled(ymd, date) {
        // Disable weekends (Sunday = `0`, Saturday = `6`) and
        // disable days that fall on the 13th of the month
        const weekday = date.getDay()
        const day = date.getDate()
        let today = (new Date())
         console.log(today.getDay())
        // Return `true` if the date should be disabled
        return weekday === 0 || weekday === 6 || day === (today - 1)
      },
         post: function(data) {
           if(this.form.name && this.form.date !=":" && this.form.time !=":" && this.form.email !="" && this.form.id !="")
      this.$http.post("http://localhost:3000/appointmentapp/appointment", data);
    },
    fetchInformation() {
      fetch(`http://localhost:3000/appointmentapp`)
        .then(res => {
          return res.json();
        })
        .then(this.setValues);
    },
    setValues(data) {
      console.log("data:",data)
      this.items = data;
    },
      onSubmit(evt) {
        evt.preventDefault()
        let data = JSON.stringify(this.form)
        this.post(data)
        alert(JSON.stringify(this.form))
      },
      onReset(evt) {
        this.fetchInformation()
        evt.preventDefault()
        // Reset our form values
        this.form.email = ''
        this.form.name = ''
       this.form.date = ""
       this.form.time =""
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
    }
    
  }
</script>