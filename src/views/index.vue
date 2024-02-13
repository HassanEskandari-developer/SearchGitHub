<template>
  <main
    class="h-[150vh] lg:h-auto lg:min-h-screen  w-full flex justify-center lg:items-center bg-[#141c30] relative py-10 lg:py-0"
  >
    <div
      v-if="isLoading"
      class="flex justify-center items-center bg-gray-400/70 absolute w-full h-full top-0 right-0 z-10"
    >
      <div class="lds-ring">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
      </div>
    </div>

    <section class="lg:w-[800px] w-[90%] h-[650px] flex flex-col gap-10">
      <!-- ////////////////////////////////////////search/////////////////////////////////////////////////////////////////////////// -->

      <div class="w-full relative">
        <input
          @keydown.enter="serchGit"
          v-model="gitSearch"
          type="text"
          class="w-full h-16 rounded-[15px] lg:px-16 px-9 text-white placeholder:lg:px-16 placeholder:px-9 bg-[#1f2a48] placeholder:text-gray-200"
          placeholder="Search gitHub UsreName"
        />
        <button
          @click="serchGit"
          class="w-14 lg:w-20 h-10 absolute top-3 right-3 flex justify-center items-center rounded-[10px] bg-blue-300 hover:bg-yellow-200"
        >
          Search
        </button>
        <span class="w-20 h-10">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="w-5 lg:w-10 h-7 text-blue-600 absolute top-4 left-3"
            viewBox="0 0 24 24"
            fill="currentColor"
          >
            <path
              d="M18.031 16.6168L22.3137 20.8995L20.8995 22.3137L16.6168 18.031C15.0769 19.263 13.124 20 11 20C6.032 20 2 15.968 2 11C2 6.032 6.032 2 11 2C15.968 2 20 6.032 20 11C20 13.124 19.263 15.0769 18.031 16.6168ZM16.0247 15.8748C17.2475 14.6146 18 12.8956 18 11C18 7.1325 14.8675 4 11 4C7.1325 4 4 7.1325 4 11C4 14.8675 7.1325 18 11 18C12.8956 18 14.6146 17.2475 15.8748 16.0247L16.0247 15.8748Z"
            ></path>
          </svg>
        </span>
      </div>

      <!-- //////////////////////found///////////////////// -->
      <p
        v-if="!gitSearch"
        class="bg-red-500 text-white flex justify-center items-center h-10 text-2xl font-bold rounded-[10px]"
      >
        Please enter your username
      </p>
      <p
        v-else-if="gitSearch && !responser"
        class="bg-red-500 text-white flex justify-center items-center h-10 text-2xl font-bold rounded-[10px]"
      >
        Please enter your username correctly
      </p>
      <div
        v-if="responser"
        class="w-full lg:h-[550px] rounded-[15px] flex bg-[#1f2a48]"
      >
        <div class="lg:p-10 p-3 flex flex-col gap-y-4 ">
          <!-- <svg
            xmlns="http://www.w3.org/2000/svg"
            class="w-20 h-20 lg:w-32 lg:h-32 border rounded-full"
            viewBox="0 0 24 24"
            fill="currentColor"
          >
            <path
              d="M5.88401 18.6533C5.58404 18.4526 5.32587 18.1975 5.0239 17.8369C4.91473 17.7065 4.47283 17.1524 4.55811 17.2583C4.09533 16.6833 3.80296 16.417 3.50156 16.3089C2.9817 16.1225 2.7114 15.5499 2.89784 15.0301C3.08428 14.5102 3.65685 14.2399 4.17672 14.4263C4.92936 14.6963 5.43847 15.1611 6.12425 16.0143C6.03025 15.8974 6.46364 16.441 6.55731 16.5529C6.74784 16.7804 6.88732 16.9182 6.99629 16.9911C7.20118 17.1283 7.58451 17.1874 8.14709 17.1311C8.17065 16.7489 8.24136 16.3783 8.34919 16.0358C5.38097 15.3104 3.70116 13.3952 3.70116 9.63971C3.70116 8.40085 4.0704 7.28393 4.75917 6.3478C4.5415 5.45392 4.57433 4.37284 5.06092 3.15636C5.1725 2.87739 5.40361 2.66338 5.69031 2.57352C5.77242 2.54973 5.81791 2.53915 5.89878 2.52673C6.70167 2.40343 7.83573 2.69705 9.31449 3.62336C10.181 3.41879 11.0885 3.315 12.0012 3.315C12.9129 3.315 13.8196 3.4186 14.6854 3.62277C16.1619 2.69 17.2986 2.39649 18.1072 2.52651C18.1919 2.54013 18.2645 2.55783 18.3249 2.57766C18.6059 2.66991 18.8316 2.88179 18.9414 3.15636C19.4279 4.37256 19.4608 5.45344 19.2433 6.3472C19.9342 7.28337 20.3012 8.39208 20.3012 9.63971C20.3012 13.3968 18.627 15.3048 15.6588 16.032C15.7837 16.447 15.8496 16.9105 15.8496 17.4121C15.8496 18.0765 15.8471 18.711 15.8424 19.4225C15.8412 19.6127 15.8397 19.8159 15.8375 20.1281C16.2129 20.2109 16.5229 20.5077 16.6031 20.9089C16.7114 21.4504 16.3602 21.9773 15.8186 22.0856C14.6794 22.3134 13.8353 21.5538 13.8353 20.5611C13.8353 20.4708 13.836 20.3417 13.8375 20.1145C13.8398 19.8015 13.8412 19.599 13.8425 19.4094C13.8471 18.7019 13.8496 18.0716 13.8496 17.4121C13.8496 16.7148 13.6664 16.2602 13.4237 16.051C12.7627 15.4812 13.0977 14.3973 13.965 14.2999C16.9314 13.9666 18.3012 12.8177 18.3012 9.63971C18.3012 8.68508 17.9893 7.89571 17.3881 7.23559C17.1301 6.95233 17.0567 6.54659 17.199 6.19087C17.3647 5.77663 17.4354 5.23384 17.2941 4.57702L17.2847 4.57968C16.7928 4.71886 16.1744 5.0198 15.4261 5.5285C15.182 5.69438 14.8772 5.74401 14.5932 5.66413C13.7729 5.43343 12.8913 5.315 12.0012 5.315C11.111 5.315 10.2294 5.43343 9.40916 5.66413C9.12662 5.74359 8.82344 5.69492 8.57997 5.53101C7.8274 5.02439 7.2056 4.72379 6.71079 4.58376C6.56735 5.23696 6.63814 5.77782 6.80336 6.19087C6.94565 6.54659 6.87219 6.95233 6.61423 7.23559C6.01715 7.8912 5.70116 8.69376 5.70116 9.63971C5.70116 12.8116 7.07225 13.9683 10.023 14.2999C10.8883 14.3971 11.2246 15.4769 10.5675 16.0482C10.3751 16.2156 10.1384 16.7802 10.1384 17.4121V20.5611C10.1384 21.5474 9.30356 22.2869 8.17878 22.09C7.63476 21.9948 7.27093 21.4766 7.36613 20.9326C7.43827 20.5204 7.75331 20.2116 8.13841 20.1276V19.1381C7.22829 19.1994 6.47656 19.0498 5.88401 18.6533Z"
            ></path>
          </svg> -->

          <img :src="responser.avatar_url" alt="" class="rounded-full" />

          <div class="flex justify-center items-center">
            <p class="text-4xl text-[#FB6087]">You Joined GitHub</p>
          </div>

          <!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

         <div class="lg:grid lg:grid-cols-3 flex flex-col lg:gap-x-5 gap-y-4 lg:gap-y-0 ">
          <div class="flex flex-col justify-center items-center gap-y-2  ">
            <div class="w-16 h-14 bg-[#343650] flex flex-col justify-center items-center relative rounded-[10px] shadow-2xl shadow-[#343650]">
           <div class="w-16 h-10 border-b-[1px] border-b-gray-500"></div>
          <span class="absolute text-4xl font-bold top-2 text-[#FB6087]">{{YearsGit}}</span>
          <div class="w-16 h-10"></div>
         </div>
         <span class="text-white text-xl">Years</span>
         </div>

         <!-- //////////////////// -->

         <div class="flex flex-col justify-center items-center gap-y-2">
            <div class="w-16 h-14 bg-[#343650] flex flex-col justify-center items-center relative rounded-[10px] shadow-2xl shadow-[#343650]">
           <div class="w-16 h-10 border-b-[1px] border-b-gray-500"></div>
          <span class="absolute text-4xl font-bold top-2 text-[#FB6087]">{{MounthGit}}</span>
          <div class="w-16 h-10"></div>
         </div>
         <span class="text-white text-xl">Mounth</span>
         </div>
         <!-- ///////////////////// -->

         <div class="flex flex-col justify-center items-center  gap-y-2">
            <div class="w-16 h-14 bg-[#343650] flex flex-col justify-center items-center relative rounded-[10px] shadow-2xl shadow-[#343650]">
           <div class="w-16 h-10 border-b-[1px] border-b-gray-500"></div>
          <span class="absolute text-4xl font-bold top-2 text-[#FB6087]">{{DayGit}}</span>
          <div class="w-16 h-10"></div>
         </div>
         <span class="text-white text-xl">Day</span>
         </div>
         <!-- //////////////////// -->
         </div>

        


          <!-- /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->
          
        </div>

        <!-- /////////////////..................../////////////////////////////// -->

        <div class="flex flex-col w-full lg:p-10 p-2 gap-y-14">
          <div class="flex flex-col">
            <div class="flex flex-col lg:flex-row lg:justify-between">
              <span class="text-white text-xl"
                ><a target="_blank" :href="responser.html_url">{{
                  responser.name
                }}</a></span
              >
              <span class="text-gray-300"
                >joined
                {{
                  new Date(responser.created_at).toLocaleDateString("en-US", {
                    day: "numeric",
                  })
                }}
                {{
                  new Date(responser.created_at).toLocaleDateString("en-US", {
                    month: "short",
                    year: "numeric",
                  })
                }}</span
              >
            </div>
            <span class="text-blue-600"
              ><a target="_blank" :href="responser.html_url">{{
                responser.login
              }}</a></span
            >
          </div>

          <div>
            <h2 class="text-gray-300 text-lg font-bold">{{ responser.bio }}</h2>
          </div>

          <!-- /////////////////////numberflow/////////////////////////////////// -->
          <div
            class=" lg:h-16 flex flex-col md:flex-row h-auto py-2 px-3 lg:pl-7 lg:gap-x-20 gap-x-3 rounded-[10px] bg-[#141c2f] gap-y-3 md:gap-y-0"
          >
            <div class="flex flex-col">
              <h2 class="text-gray-400">Repositories</h2>
              <span class="text-white">{{ responser.public_repos }}</span>
            </div>
            <div class="flex flex-col">
              <h2 class="text-gray-400">Followers</h2>
              <span class="text-white">{{ responser.followers }}</span>
            </div>
            <div class="flex flex-col">
              <h2 class="text-gray-400">Following</h2>
              <span class="text-white">{{ responser.following }}</span>
            </div>
          </div>
          <!-- //////////////////////////////link......///////////////// -->

          <div class="flex flex-col lg:grid lg:grid-cols-2 gap-4">
            <div class="flex gap-3">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="w-6 h-6 text-white"
                viewBox="0 0 24 24"
                fill="currentColor"
              >
                <path
                  d="M12 23.7279L5.63604 17.364C2.12132 13.8492 2.12132 8.15076 5.63604 4.63604C9.15076 1.12132 14.8492 1.12132 18.364 4.63604C21.8787 8.15076 21.8787 13.8492 18.364 17.364L12 23.7279ZM16.9497 15.9497C19.6834 13.2161 19.6834 8.78392 16.9497 6.05025C14.2161 3.31658 9.78392 3.31658 7.05025 6.05025C4.31658 8.78392 4.31658 13.2161 7.05025 15.9497L12 20.8995L16.9497 15.9497ZM12 13C10.8954 13 10 12.1046 10 11C10 9.89543 10.8954 9 12 9C13.1046 9 14 9.89543 14 11C14 12.1046 13.1046 13 12 13Z"
                ></path>
              </svg>
              <p class="text-gray-300">
                {{ responser.location ? responser.location : "-" }}
              </p>
            </div>
            <!-- ///////////////////////////// -->
            <div class="flex gap-3">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="w-6 h-6 text-white"
                viewBox="0 0 24 24"
                fill="currentColor"
              >
                <path
                  d="M15.3499 5.55005C13.7681 5.55005 12.4786 6.81809 12.4504 8.39658L12.4223 9.97162C12.4164 10.3029 12.143 10.5667 11.8117 10.5608C11.7881 10.5604 11.7646 10.5586 11.7413 10.5554L10.1805 10.3426C8.12699 10.0625 6.15883 9.11736 4.27072 7.54411C3.67275 10.8538 4.84 13.1472 7.65342 14.916L9.40041 16.0142C9.68095 16.1906 9.7654 16.561 9.58903 16.8415C9.54861 16.9058 9.49636 16.9619 9.43504 17.0067L7.84338 18.1696C8.78973 18.229 9.68938 18.1875 10.435 18.0387C15.1526 17.0973 18.2897 13.547 18.2897 7.69109C18.2897 7.213 17.2774 5.55005 15.3499 5.55005ZM10.4507 8.3609C10.4983 5.69584 12.6735 3.55005 15.3499 3.55005C16.7132 3.55005 17.9465 4.10683 18.8348 5.0054C19.5462 5.00005 20.1514 5.17991 21.5035 4.35967C21.1693 6.00005 21.0034 6.71201 20.2897 7.69109C20.2897 15.3326 15.5926 19.0489 10.8264 20C7.5587 20.6522 2.80646 19.5815 1.44531 18.1587C2.13874 18.1054 4.95928 17.802 6.58895 16.6092C5.20994 15.6987 -0.278631 12.4681 3.32772 3.78642C5.02119 5.76307 6.73797 7.10855 8.47807 7.82286C9.63548 8.29798 9.91978 8.2885 10.4507 8.3609Z"
                ></path>
              </svg>
              <p class="text-gray-300">
                {{
                  responser.twitter_username ? responser.twitter_username : "-"
                }}
              </p>
            </div>
            <!-- /////////////////////////// -->
            <div class="flex gap-3">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="w-6 h-6 text-white"
                viewBox="0 0 24 24"
                fill="currentColor"
              >
                <path
                  d="M13.0607 8.11097L14.4749 9.52518C17.2086 12.2589 17.2086 16.691 14.4749 19.4247L14.1214 19.7782C11.3877 22.5119 6.95555 22.5119 4.22188 19.7782C1.48821 17.0446 1.48821 12.6124 4.22188 9.87874L5.6361 11.293C3.68348 13.2456 3.68348 16.4114 5.6361 18.364C7.58872 20.3166 10.7545 20.3166 12.7072 18.364L13.0607 18.0105C15.0133 16.0578 15.0133 12.892 13.0607 10.9394L11.6465 9.52518L13.0607 8.11097ZM19.7782 14.1214L18.364 12.7072C20.3166 10.7545 20.3166 7.58872 18.364 5.6361C16.4114 3.68348 13.2456 3.68348 11.293 5.6361L10.9394 5.98965C8.98678 7.94227 8.98678 11.1081 10.9394 13.0607L12.3536 14.4749L10.9394 15.8891L9.52518 14.4749C6.79151 11.7413 6.79151 7.30911 9.52518 4.57544L9.87874 4.22188C12.6124 1.48821 17.0446 1.48821 19.7782 4.22188C22.5119 6.95555 22.5119 11.3877 19.7782 14.1214Z"
                ></path>
              </svg>
              <p class="text-gray-300">
                {{ responser.blog ? responser.blog : "-" }}
              </p>
            </div>

            <!-- /////////////////////////// -->
            <div class="flex gap-3">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="w-5 h-5 text-white"
                viewBox="0 0 24 24"
                fill="currentColor"
              >
                <path
                  d="M21 19H23V21H1V19H3V4C3 3.44772 3.44772 3 4 3H14C14.5523 3 15 3.44772 15 4V19H19V11H17V9H20C20.5523 9 21 9.44772 21 10V19ZM5 5V19H13V5H5ZM7 11H11V13H7V11ZM7 7H11V9H7V7Z"
                ></path>
              </svg>
              <p class="text-gray-300">
                {{ responser.company ? responser.company : "-" }}
              </p>
            </div>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script>
import { Octokit } from "octokit";
export default {
  data() {
    return {
      gitSearch: "",
      responser: null,
      allDate: "",
      isLoading: false,
      d: "",
      m: "",
      y: "",
      year: "",
      month: "",
      day: "",
    };
  },
  computed: {
    DayGit() {
      this.gitDateFunc()
      //  return `You joined github ${this.y} years and ${this.m} months and ${this.d} day ago`;
       return `${this.d}`;
    },
    MounthGit() {
      this.gitDateFunc()
      //  return `You joined github ${this.y} years and ${this.m} months and ${this.d} day ago`;
       return `${this.m}`;
    },
    YearsGit() {
      this.gitDateFunc()
      //  return `You joined github ${this.y} years and ${this.m} months and ${this.d} day ago`;
      //  return `${this.y}`;
     
        // return this.y
        if(this.y==0){
          this.y=''
        }else{
          return this.y
        }
      
    },
  },
  methods: {
    async serchGit() {
      try {
        if (this.gitSearch && this.gitSearch.length > 0) {
          this.responser = null;
          const octokit = new Octokit({
            auth: "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",
          });
          this.isLoading = true;
          const response = await octokit.request(
            `GET /users/${this.gitSearch}`,
            {
              username: "USERNAME",
              headers: {
                "X-GitHub-Api-Version": "2022-11-28",
              },
            }
          );
          this.responser = response.data;
          this.day = new Date(this.responser.created_at).getDate();
          this.month = new Date(this.responser.created_at).getMonth() + 1;
          this.year = new Date(this.responser.created_at).getFullYear();
          console.log(response);
        }
      } catch (error) {
        console.log(error.response);
      } finally {
        this.isLoading = false;
      }
    },

    gitDateFunc(){
        let nowTime = new Date();
      let d1 = nowTime.getDate();
      let m1 = nowTime.getMonth() + 1;
      let y1 = nowTime.getFullYear();
      let allMonth = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
      if (this.day > d1) {
        d1 = d1 + allMonth[m1 - 1];
        m1 = m1 - 1;
      }
      if (this.month > m1) {
        m1 = m1 + 12;
        y1 = y1 - 1;
      }

      this.d = d1 - this.day;
      this.m = m1 - this.month;
      this.y = y1 - this.year;
     
    }
  },
};
</script>

<style>
body,
body *,
* {
  font-family: "Inter", sans-serif;
  font-optical-sizing: auto;
  font-weight: 500;
  font-style: normal;
  font-variation-settings: "slnt" 0;
}

.lds-ring {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}
.lds-ring div {
  box-sizing: border-box;
  display: block;
  position: absolute;
  width: 64px;
  height: 64px;
  margin: 8px;
  border: 8px solid #fff;
  border-radius: 50%;
  animation: lds-ring 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
  border-color: #fff transparent transparent transparent;
}
.lds-ring div:nth-child(1) {
  animation-delay: -0.45s;
}
.lds-ring div:nth-child(2) {
  animation-delay: -0.3s;
}
.lds-ring div:nth-child(3) {
  animation-delay: -0.15s;
}
@keyframes lds-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>