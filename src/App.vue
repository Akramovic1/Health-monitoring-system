<template>
  <v-app>
    <nav>
        <div class="menu">
            <p class="website_name">BIGDATA Milestone 2
              <!-- <img src="../src/bigdata.png" alt="logo" style="width:80px"> -->
              </p>
            <div class="menu_links">
                <a  @click = "myToggleCPU(CPUService)" class="link">CPU Health</a>
                <a  @click = "myToggleDISK(DiskService)" class="link">DISK Health</a>
                <a  @click = "myToggleRAM(RAMService)" class="link">RAM Health</a>
                <a  @click = "myTogglePEAK(PeakService)" class="link">PEAK Health</a>
                <a  @click = "myToggleCOUNT(CountService)" class="link">Count Service</a>
                <div class="text-xs-center">
                    <v-dialog
                    v-model="dialog"
                    width="500"
                    hight="500"
                    >

                    <v-card>
                        <v-card-title
                        class="headlin lighten-2"
                        primary-title
                        >
                        {{this.Title}}
                        </v-card-title>

                        <v-card-text>
                        {{this.singleContent}}
                        </v-card-text>

                        <v-divider></v-divider>

                        <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn
                            color="primary"
                            flat
                            @click="dialog = false"
                        >
                            OK
                        </v-btn>
                        </v-card-actions>
                    </v-card>
                    </v-dialog>
                </div>
            </div>
            <div class="menu_icon">
                <span class="icon"></span>
            </div>
        </div>
    </nav>

    <section class="wrapper">

        <div class="container">

            <div id="scene" class="scene" data-hover-only="false">
                <div class="circle" data-depth="1.2"></div>
                <div class="one" data-depth="0.9">
                    <div class="content">
                        <span class="piece"></span>
                        <span class="piece"></span>
                        <span class="piece"></span>
                    </div>
                </div>
                <div class="two" data-depth="0.60">
                    <div class="content">
                        <span class="piece"></span>
                        <span class="piece"></span>
                        <span class="piece"></span>
                    </div>
                </div>
                <div class="three" data-depth="0.40">
                    <div class="content">
                        <span class="piece"></span>
                        <span class="piece"></span>
                        <span class="piece"></span>
                    </div>
                </div>
                <p class="p404" data-depth="0.50">HMS</p>
                <p class="p404" data-depth="0.10">HMS</p>
            </div>
            <div class="text">
                <article>
                    <p>
                        <v-container >
                            <v-row>
                            <v-col
                                cols="12"
                                lg="6"
                            >
                                <v-menu
                                ref="menu1"
                                v-model="menu1"
                                :close-on-content-click="true"
                                transition="scale-transition"
                                offset-y
                                max-width="290px"
                                min-width="auto"
                                >
                                <template v-slot:activator="{ on, attrs }"
                                 >
                                    <v-text-field
                                    dark
                                    color="#FB8A8A"
                                    v-model="fromdate"
                                    label="Start Date"
                                    hint="DD/MM/YYYY format"
                                    persistent-hint
                                    prepend-icon="mdi-calendar"
                                    v-bind="attrs"
                                    @blur="date = parseDate(dateFormatted)"
                                    v-on="on"
                                    ></v-text-field>
                                </template>
                                <v-date-picker
                                    color="#FB8A8A"
                                    v-model="fromdate"
                                    no-title
                                    @input="menu1 = true"
                                ></v-date-picker>
                                </v-menu>
                            </v-col>

                            <v-col
                                color="#FB8A8A"
                                cols="12"
                                lg="6"
                            >
                                <v-menu
                                color="#FB8A8A"
                                v-model="menu2"
                                :close-on-content-click="true"
                                transition="scale-transition"
                                offset-y
                                max-width="290px"
                                min-width="auto"
                                >
                                <template v-slot:activator="{ on, attrs }" color="#FB8A8A">
                                    <v-text-field
                                    dark
                                    color="#FB8A8A"
                                    v-model="todate"
                                    label="End Date"
                                    hint="DD/MM/YYYY format"
                                    persistent-hint
                                    prepend-icon="mdi-calendar"
                                    v-bind="attrs"
                                    v-on="on"
                                    ></v-text-field>
                                </template>
                                <v-date-picker
                                    color="#FB8A8A"
                                    v-model="todate"
                                    no-title
                                    @input="menu2 = true"
                                ></v-date-picker>
                                </v-menu>
                            </v-col>
                            </v-row>
                        </v-container>
                    </p>
                    <button @click="myToggleFULL(FullReport)">FULL Report</button>
                </article>
            </div>

        </div>
    </section>
  </v-app>
</template>

<script>
import axios from "axios"
export default {
  name: 'App',

  components: {
  },
    methods:{
        myToggleFULL:function(obj){

          // this.da=this.myToggleCPU();
          // this.da+=this.myToggleRAM();
          // this.da+=this.myToggleDISK();
          // this.da+=this.myTogglePEAK();
          // this.da+=this.myToggleCOUNT();
          // this.singleContent = this.da
          // this.dialog = !this.dialog;
          // this.Title = obj.title;

          axios.get("http://localhost:8080/api/get", {
            params: {
              type: "CPU",
              startDate:this.fromdate,
              endDate:this.todate,
            },
          })
              .then((Response) => {
                const Data = Response.data
                this.singleContent = Data
                this.dialog = !this.dialog;
                this.Title = obj.title;
                return Data;
              })
          //   if(obj.title == "Full Report"){
          //     this.singleContent = this.CPUService.content + this.RAMService.content + this.DiskService.content + this.PeakService.content;
          //   }else if(obj.title == "CPU Service"){
          //     this.singleContent = obj.content;
          //   }else if(obj.title == "RAM Service"){
          //     this.singleContent = obj.content;
          //   }else if(obj.title == "Disk Service"){
          //     this.singleContent = obj.content;
          //   }else if(obj.title == "Peak Service"){
          //     this.singleContent = obj.content;
          //   }else if(obj.title == "Count Service"){
          //     this.singleContent = obj.content;
          //   }
        },
      myToggleCPU:function(obj){
        axios.get("http://localhost:8080/api/get", {
          params: {
            type: "CPU",
            startDate:this.fromdate,
            endDate:this.todate,
          },
        })
            .then((Response) => {
              const Data = Response.data
              this.singleContent = Data
              this.dialog = !this.dialog;
              this.Title = obj.title;
              return Data;
            })

      },
      myToggleRAM:function(obj){
        axios.get("http://localhost:8080/api/get", {
          params: {
            type: "RAM",
            startDate:this.fromdate,
            endDate:this.todate,
          },
        })
            .then((Response) => {
              const Data = Response.data
              this.singleContent = Data
              this.dialog = !this.dialog;
              this.Title = obj.title;
              return Data;
            })
      },
      myToggleDISK:function(obj){
        axios.get("http://localhost:8080/api/get", {
          params: {
            type: "DISK",
            startDate:this.fromdate,
            endDate:this.todate,
          },
        })
            .then((Response) => {
              const Data = Response.data
              this.singleContent = Data
              this.dialog = !this.dialog;
              this.Title = obj.title;
              return Data;
            })
      },
      myTogglePEAK:function(obj){
        axios.get("http://localhost:8080/api/get", {
          params: {
            type: "ForEach",
            startDate:this.fromdate,
            endDate:this.todate,
          },
        })
            .then((Response) => {
              const Data = Response.data
              this.singleContent = Data
              this.dialog = !this.dialog;
              this.Title = obj.title;
              return Data;
            })
      },
      myToggleCOUNT:function(obj){
        axios.get("http://localhost:8080/api/get", {
          params: {
            type: "Count",
            startDate:this.fromdate,
            endDate:this.todate,
          },
        })
            .then((Response) => {
              const Data = Response.data
              this.singleContent = Data
              this.dialog = !this.dialog;
              this.Title = obj.title;
              return Data;
            })
      },
    },
 mounted: function() {
   const Parallax = require('parallax-js')
   var scene = document.getElementById('scene');
   //eslint-disable-next-line
   var parallax = new Parallax(scene);

 },
  data: () => ({
    todate:'',
    fromdate:'',
    da:'',
    dialog: false,
    Title:'',
    CPUService:{
        title: 'CPU Service',
        content: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
    },
    RAMService:{
        title: 'RAM Service',
        content: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
    },
    DiskService:{
        title: 'Disk Service',
        content: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
    },
    PeakService:{
        title: 'Peak Service',
        content: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
    },
    FullReport:{
        title: "Full Report",
    },CountService:{
      title: "Count Service",
      content: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
    }
    //
  }),
};
</script>
<style scoped lang="scss">

@import url("https://fonts.googleapis.com/css?family=Barlow+Condensed:300,400,500,600,700,800,900|Barlow:300,400,500,600,700,800,900&display=swap");

$font-01: "Barlow",
sans-serif;
$font-02: "Barlow Condensed",
sans-serif;

$m-01: #FB8A8A;
$m-02: #FFEDC0;

$bg-01: #695681;
$bg-02: #36184F;
$bg-03: #32243E;

$g-01: linear-gradient(90deg, #FFEDC0 0%, #FF9D87 100%);
$g-02: linear-gradient(90deg, #8077EA 13.7%, #EB73FF 94.65%);

$cubic: cubic-bezier(0.4, 0.35, 0, 1.53);
$cubic2: cubic-bezier(0.18, 0.89, 0.32, 1.15);

$circleShadow: inset 5px 20px 40px rgba($bg-02, 0.25),
inset 5px 0px 5px rgba($bg-03, 0.3),
inset 5px 5px 20px rgba($bg-03, 0.25),
2px 2px 5px rgba(white, 0.2);


@mixin sm {
    @media screen and (max-width: 799px) {
        @content;
    }
}

@mixin height {
    @media screen and (max-height:660px) {
        @content;
    }
}

h1,
h2,
h3,
h4,
h5,
h6,
p,
ul,
li,
button,
a,
i,
input,
body {
    margin: 0;
    padding: 0;
    list-style: none;
    border: 0;
    -webkit-tap-highlight-color: transparent;
    text-decoration: none;
    color: inherit;

    &:focus {
        outline: 0;
    }
}


body {
    margin: 0;
    padding: 0;
    height: auto;
    font-family: $font-01;
    background: $bg-01;

}

.logo {
    position: fixed;
    z-index: 5;
    bottom: 10px;
    right: 10px;
    width: 40px;
    height: 40px;
    border-radius: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    background: rgba(0, 0, 0, 0.1);
    border-radius: 100%;
    backdrop-filter: blur(5px);

    img {

        width: 55%;
        height: 55%;
        transform: translateY(-1px);
        opacity: 0.8;
    }
}


nav {
    .menu {
        width: 100%;
        height: 80px;
        position: absolute;
        display: flex;
        align-items: center;
        justify-content: space-between;
        //background-color: red;
        padding: 0 5%;
        box-sizing: border-box;
        z-index: 3;

        .website_name {
            color: $bg-01;
            font-weight: 600;
            font-size: 20px;
            letter-spacing: 1px;
            background: white;
            padding: 4px 8px;
            border-radius: 2px;
            opacity: 0.5;
            transition: all 0.4s ease;
            cursor: pointer;

            &:hover {
                opacity: 1;
            }
        }

        .menu_links {
            transition: all 0.4s ease;
            opacity: 0.9;

            &:hover {
                opacity: 1;
            }

            @include sm {
                display: none;

            }

            .link {
                color: white;
                text-transform: uppercase;
                font-weight: 500;
                margin-right: 50px;
                letter-spacing: 2px;
                position: relative;
                transition: all 0.3s 0.2s ease;

                &:last-child {
                    margin-right: 0;
                }

                &:before {
                    content: '';
                    position: absolute;
                    width: 0px;
                    height: 4px;
                    background: $g-01;
                    bottom: -10px;
                    border-radius: 4px;
                    transition: all 0.4s cubic-bezier(0.82, 0.02, 0.13, 1.26);
                    left: 100%;
                }

                &:hover {
                    opacity: 1;
                    color: $m-01;

                    &:before {
                        width: 100%;
                        left: 0;
                    }
                }
            }
        }

        .menu_icon {
            width: 40px;
            height: 40px;
            position: relative;
            display: none;
            justify-content: center;
            align-items: center;
            cursor: pointer;


            @include sm {
                display: flex;
            }

            .icon {
                width: 24px;
                height: 2px;
                background: white;
                position: absolute;

                &:before,
                &:after {
                    content: '';
                    width: 100%;
                    height: 100%;
                    background: inherit;
                    position: absolute;
                    transition: all 0.3s cubic-bezier(0.49, 0.04, 0, 1.55);
                }

                &:before {
                    transform: translateY(-8px);
                }

                &:after {
                    transform: translateY(8px);
                }


            }

            &:hover {
                .icon {
                    background: $m-02;

                    &:before {
                        transform: translateY(-10px);
                    }

                    &:after {
                        transform: translateY(10px);
                    }
                }
            }
        }
    }
}

.wrapper {
    display: grid;
    grid-template-columns: 1fr;
    justify-content: center;
    align-items: center;
    height: 100vh;
    overflow-x: hidden;
    background-color: rgb(67, 172, 233);


    .container {
        margin: 0 auto;
        transition: all 0.4s ease;
        display: flex;
        justify-content: center;
        align-items: center;
        position: relative;

        // Scene for the Parallax Effect
        .scene {
            position: absolute;
            width: 100vw;
            height: 100vh;
            vertical-align: middle;
        }

        // All elements' containers
        .one,
        .two,
        .three,
        .circle,
        .p404 {
            width: 60%;
            height: 60%;
            top: 20% !important;
            left: 20% !important;
            min-width: 400px;
            min-height: 400px;

            .content {
                width: 400px;
                height: 600px;
                display: flex;
                justify-content: center;
                align-items: center;
                position: absolute;
                top: 50%;
                left: 50%;
                transform: translate(-50%, -50%);
                animation: content 0.8s cubic-bezier(1, 0.06, 0.25, 1) backwards;

                @keyframes content {
                    0% {
                        width: 0;
                    }
                }

                // Pieces
                .piece {
                    width: 200px;
                    height: 80px;
                    display: flex;
                    position: absolute;
                    border-radius: 80px;
                    z-index: 1;

                    animation: pieceLeft 8s cubic-bezier(1, 0.06, 0.25, 1) infinite both;


                    @keyframes pieceLeft {
                        0% {}

                        50% {
                            left: 80%;
                            width: 10%;
                        }

                        100% {}

                    }

                    @keyframes pieceRight {
                        0% {}

                        50% {
                            right: 80%;
                            width: 10%;
                        }

                        100% {}

                    }

                }
            }

            @include sm {
                width: 90%;
                height: 90%;
                top: 5% !important;
                left: 5% !important;
                min-width: 280px;
                min-height: 280px;
            }

            @include height {
                min-width: 280px;
                min-height: 280px;
                width: 60%;
                height: 60%;
                top: 20% !important;
                left: 20% !important;
            }
        }

        // Text and Button container
        .text {
            width: 60%;
            height: 30%;
            min-width: 400px;
            min-height: 500px;
            position: absolute;
            margin: 60px 0;
            animation: text 0.6s 1.8s ease backwards;

            @keyframes text {
                0% {
                    opacity: 0;
                    transform: translateY(40px);
                }
            }

            @include sm {
                min-height: 400px;
                height: 80%;
            }

            article {
                width: 400px;
                position: absolute;
                bottom: 0;
                z-index: 4;
                display: flex;
                flex-direction: column;
                justify-content: center;
                align-items: center;
                text-align: center;
                bottom: 0;
                left: 50%;
                transform: translateX(-50%);


                @include sm {
                    width: 100%;
                }

                p {
                    font-size: 18px;
                    letter-spacing: 0.6px;
                    margin-bottom: 40px;
                    text-shadow: 6px 6px 10px $bg-03;
                }

                button {
                    height: 40px;
                    padding: 0 30px;
                    border-radius: 50px;
                    cursor: pointer;
                    box-shadow: 0px 15px 20px rgba($bg-02, 0.5);
                    z-index: 3;
                    color: $bg-01;
                    background-color: white;
                    text-transform: uppercase;
                    font-weight: 600;
                    font-size: 12px;
                    transition: all 0.3s ease;


                    &:hover {
                        box-shadow: 0px 10px 10px -10px rgba($bg-02, 0.5);
                        transform: translateY(5px);
                        background: $m-01;
                        color: white;
                    }
                }
            }
        }

        // The 404 Number
        .p404 {
            font-size: 200px;
            font-weight: 700;
            letter-spacing: 4px;
            color: white;
            display: flex !important;
            justify-content: center;
            align-items: center;
            position: absolute;
            z-index: 2;
            animation: anime404 0.6s cubic-bezier(0.3, 0.8, 1, 1.05) both;
            animation-delay: 1.2s;

            @include sm {
                font-size: 100px;
            }

            @keyframes anime404 {
                0% {
                    opacity: 0;
                    transform: scale(10) skew(20deg, 20deg);
                }
            }

            &:nth-of-type(2) {
                color: $bg-02;
                z-index: 1;
                animation-delay: 1s;
                filter: blur(10px);
                opacity: 0.8;
            }


        }

        // Bigger Circle
        .circle {
            position: absolute;

            &:before {
                content: '';
                position: absolute;
                width: 800px;
                height: 800px;
                background-color: rgba($bg-02, 0.2);
                border-radius: 100%;
                top: 50%;
                left: 50%;
                transform: translate(-50%, -50%);
                box-shadow: $circleShadow;
                animation: circle 0.8s cubic-bezier(1, 0.06, 0.25, 1) backwards;


                @keyframes circle {
                    0% {
                        width: 0;
                        height: 0;
                    }
                }

                @include sm {
                    width: 400px;
                    height: 400px;
                }
            }
        }

        // Container 1
        .one {
            .content {

                // Smaller Circle
                &:before {
                    content: '';
                    position: absolute;
                    width: 600px;
                    height: 600px;
                    background-color: rgba($bg-02, 0.3);
                    border-radius: 100%;
                    box-shadow: $circleShadow;
                    animation: circle 0.8s 0.4s cubic-bezier(1, 0.06, 0.25, 1) backwards;

                    @include sm {
                        width: 300px;
                        height: 300px;
                    }
                }

                .piece {
                    background: $g-02;

                    &:nth-child(1) {
                        right: 15%;
                        top: 18%;
                        height: 30px;
                        width: 120px;
                        animation-delay: 0.5s;
                        animation-name: pieceRight;
                    }

                    &:nth-child(2) {
                        left: 15%;
                        top: 45%;
                        width: 150px;
                        height: 50px;
                        animation-delay: 1s;
                        animation-name: pieceLeft;
                    }

                    &:nth-child(3) {
                        left: 10%;
                        top: 75%;
                        height: 20px;
                        width: 70px;
                        animation-delay: 1.5s;
                        animation-name: pieceLeft;
                    }

                }


            }
        }

        // Container 2
        .two {
            .content {
                .piece {
                    background: $g-01;

                    &:nth-child(1) {
                        left: 0%;
                        top: 25%;
                        height: 40px;
                        width: 120px;
                        animation-delay: 2s;
                        animation-name: pieceLeft;
                    }

                    &:nth-child(2) {
                        right: 15%;
                        top: 35%;
                        width: 180px;
                        height: 50px;
                        animation-delay: 2.5s;
                        animation-name: pieceRight;
                    }

                    &:nth-child(3) {
                        right: 10%;
                        top: 80%;
                        height: 20px;
                        width: 160px;
                        animation-delay: 3s;
                        animation-name: pieceRight;
                    }

                }
            }
        }

        // Container 3
        .three {
            .content {
                .piece {
                    background: $m-01;

                    &:nth-child(1) {
                        left: 25%;
                        top: 35%;
                        height: 20px;
                        width: 80px;
                        animation-name: pieceLeft;
                        animation-delay: 3.5s;
                    }

                    &:nth-child(2) {
                        right: 10%;
                        top: 55%;
                        width: 140px;
                        height: 40px;
                        animation-name: pieceRight;
                        animation-delay: 4s;
                    }

                    &:nth-child(3) {
                        left: 40%;
                        top: 68%;
                        height: 20px;
                        width: 80px;
                        animation-name: pieceLeft;
                        animation-delay: 4.5s;
                    }

                }


            }
        }


    }
}
</style>
