<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div class="formSection">
      <div>
        <label>Add your skills</label>
        <input type="text" id="skill" v-model="newSkill" />
      </div>
      <div class="addButton">
        <button @click="addSkill()">ADD</button>
      </div>
    </div>

    <h2>Your Skills</h2>
    <ul>
        <li v-bind="skills" v-for="(skill, index) in skills"  :key="index">
          {{ skill.name }}
        </li>
        <li v-if="skills.length === 0">
          No Skills added.
        </li>
    </ul>

    <h2>Elgible for</h2>
    <ul>
        <li v-bind="eligibleCompany" v-for="(company, index) in eligibleCompany"  :key="index">
          {{ company.name }}
        </li>
        <li v-if="eligibleCompany.length === 0">
          No Company.
        </li>
    </ul>

    <h2>Not Eligible for</h2>
    <ul>
        <li v-bind="notEligibleCompany" v-for="(company, index) in notEligibleCompany"  :key="index">
          {{ company.name }}
        </li>
        <li v-if="notEligibleCompany.length === 0">
          No Company.
        </li>
    </ul>
  </div>
</template>

<script>
import data from '../data/data.json';
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      newSkill: '',
      skills: [],
      skillArray: [],
      eligibleCompany: [],
      notEligibleCompany: [],
      compannyRequirement: data
    };
  },
  created() {
    this.eligibilityTest();
  },
  methods: {
    addSkill() {
         if(this.skillArray.indexOf(this.newSkill) < 0) {
           this.skillArray.push(this.newSkill);
           this.skills.push({name: this.newSkill});
           this.newSkill = null;
           this.eligibilityTest();
        }
    },
    eligibilityTest() {
      if(this.skills.length) {
        this.eligibleCompany = [];
        this.notEligibleCompany = [];
        for (let i=0; i<this.compannyRequirement.length;i++) {
          let eligibleCheck = true;
          for (let j=0;j<this.compannyRequirement[i].requirements.length;j++) {
            if(this.skillArray.indexOf(this.compannyRequirement[i].requirements[j]) < 0) {
              eligibleCheck = false;
            }
          }
          if(eligibleCheck) {
            this.eligibleCompany.push({name: this.compannyRequirement[i].name});
          } else {
            this.notEligibleCompany.push({name: this.compannyRequirement[i].name});
          }
        }
      } else {
        this.notEligibleCompany = this.compannyRequirement;
      }
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: circle;
  padding: 0;
}
li {
  display: block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.formSection {
  margin: 10px;
  padding: 10px;
}
.formSection input {
  margin-left: 10px;
}
.addButton {
  margin: 10px;
}
</style>
