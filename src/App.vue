<template>
  <div id="app">
    <div class="container">
      <!-- Greeting Message -->
      <div class="greeting">Manage Your Today Activities</div>
      <!-- Form To add Activity in aList -->
      <form @submit.prevent="addActivity">
        <label for="activity">Write an Activity</label>
        <input
          type="text"
          id="activity"
          v-model="inputActivity"
          placeholder="e.g: make a call"
        />

        <input type="submit" value="Add Activity" />
      </form>
      <!-- Show all Activities -->
      <ul class="activities" v-if="activities.length > 0">
        <li v-for="item in activities" :key="item.title">
          <div class="name">{{ item.title }}</div>
          <div class="toggle-btn">
            <div class="trash">
              <button @click="removeActivity(item)">
                <img src="@/assets/trash.png" alt="icon" />
              </button>
            </div>
            <div>
              <label class="switch">
                <input
                  type="checkbox"
                  v-model="item.is_complete"
                  @change="activityComplete(item, item.is_complete)"
                />
                <span class="slider round"></span>
              </label>
            </div>
          </div>
        </li>
      </ul>
      <div class="no-result" v-else>No Activity Added Yet</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      inputActivity: "",
      activities: [],
    };
  },
  mounted() {
    this.activities = JSON.parse(localStorage.getItem("Activities")) || [];
  },
  methods: {
    addActivity() {
      if (this.inputActivity.trim() !== "") {
        let newActivity = {
          title: this.inputActivity,
          is_complete: false,
          created_at: new Date().getTime(),
        };

        this.activities.push(newActivity);

        this.inputActivity = "";
      }
    },
    activityComplete(activity, complete) {
      this.activities.map((item) => {
        if (item.title === activity.title) {
          item.is_complete = complete;
        }
      });
    },
    removeActivity(activity) {
      this.activities.map((item) => {
        if (item === activity) {
          this.activities.pop(activity);
        }
      });
    },
  },
  watch: {
    activities: {
      handler(newVal) {
        localStorage.setItem("Activities", JSON.stringify(newVal));
      },
      deep: true,
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700;800;900&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.container {
  max-width: 700px;
  width: 100%;
  margin: auto;
  border: 1px solid #eee;
  border-radius: 10px;
  padding: 20px;
  font-family: "Poppins", sans-serif;
  background-color: #f0f0f0;
}
.greeting {
  font-size: 25px;
  font-weight: 600px;
  line-height: 1.3;
}

form {
  margin: 25px 0;
}
form label {
  width: 100%;
  display: block;
  margin-bottom: 10px;
  font-size: 14px;
  font-weight: 500;
  line-height: 1.3;
}
form #activity {
  width: 100%;
  height: 35px;
  border-radius: 5px;
  border: 1px solid #ddd;
  padding: 0 10px;
  margin-bottom: 10px;
}

form #activity:focus,
form #activity:focus-visible {
  outline: 0;
}

form input[type="submit"] {
  font-size: 14px;
  font-weight: 600;
  color: #ffffff;
  background-color: brown;
  border: 0;
  border-radius: 5px;
  padding: 8px 12px;
  cursor: pointer;
}

ul.activities {
  list-style: none;
}
ul.activities li {
  background-color: #ffffff;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.3);
  border-radius: 5px;
  margin-bottom: 15px;
  padding: 10px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
ul.activities li .name {
  font-size: 16px;
  font-weight: 600;
  /* margin-bottom: 5px; */
  width: calc(100% - 110px);
}

.no-result {
  width: 100%;
  text-align: center;
  font-weight: 500;
  font-size: 14px;
  color: #111;
}
.toggle-btn {
  width: 100px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.switch {
  position: relative;
  display: inline-block;
  width: 53px;
  height: 25px;
}

/* Hide default HTML checkbox */
.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

/* The slider */
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 18px;
  width: 18px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

input:checked + .slider {
  background-color: #2196f3;
}

input:focus + .slider {
  box-shadow: 0 0 1px #2196f3;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}

.trash button {
  background: transparent;
  cursor: pointer;
  border: 0;
}
</style>