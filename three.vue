<template>
    <div class="three">
    <scroll>

    </scroll>
        <div class="activity">
            <div class="activity_content" ref="lists" >
                <div class="box" v-for="(n,i) in lists " :key="i"> {{n.title}}</div>
                <div style="text-align: center; width: 100%;height: 50px;margin: auto;background:#fff;line-height: 50px;" v-if="isShow">
                    <img src="../assets/image/loading.gif" alt="" width="100"></div>
                <div  v-else style="text-align: center; width: 100%;height: 50px;">-- 到底了哦 --</div>
            </div>
        </div>
    </div>

</template>

<script>
    import scroll from '../components/scroll'
    export default {
        name: "three",
        components: {scroll},
        created() {
            this.$store.commit('set_current', 1);
            this.getInfo();
        },
        data() {
            return {
                lists: [],
                page: 1,
                isTrue: false,
                isShow: false,
            }
        },
        methods: {
            getInfo() {
                this.$http({
                    method: "post",
                    url: "http://192.168.0.97/changlu/home/information/informationList?size=4",
                    dataType: "json",
                    data: {
                        cid: 1,
                        p: this.page
                    },
                })
                    .then(res => {
                        if (res.data.status == 1) {
                            console.log(res)
                            this.lists = this.lists.concat(res.data.result.param);
                            if (res.data.result.param.length < 4) {
                                this.isTrue = false;
                                this.isShow = false;
                            } else {
                                this.isTrue = true;
                                this.isShow = true;
                            }
                        }
                    })
                    .catch(err => {
                        console.log(err);
                    })
            },
            /*  getHeight() {
                  this.$http({
                      method: "post",
                      url: "http://192.168.0.98/changlu/home/information/informationList?size=4",
                      dataType: "json",
                      data: {
                          cid: 1,
                          p:this.page
                      },
                  })
                      .then(res => {
                          if (res.data.status == 1) {
                             this.lists=this.lists.concat(res.data.result.param);
                          }
                      })
                      .catch(err => {
                          console.log(err);
                      })

              },*/
            handleScroll() {
                console.log('進入滾動事件監聽');
                var that = this;
                 let s_top = document.documentElement.scrollTop;
                 let d_height = document.body.clientHeight-document.documentElement.clientHeight -10;
                 console.log('可视高度'+s_top,'超出可视高度'+d_height);
                 if(that.isTrue){
                     if(s_top >= d_height){
                         console.log('可以加载数据了')
                         this.isShow = true;
                         setTimeout(()=>{
                             that.page++;
                             // alert('ded');
                             console.log(that.page)
                             that.getInfo();
                         },1000);
                         that.isTrue=false;
                     }
                 }



                //判断滚动到底部
                /*if ($(window).scrollTop() >= $(document).height() - $(window).height()) {
                    if (that.isTrue) {
                        setTimeout(function() {
                        }, 1000);
                        that.isTrue = false;
                    }
                }*/

            }


        },
        mounted() {
            window.addEventListener('scroll',this.handleScroll) ;
            // window.addEventListener('touchmove', this.handleScroll)


        },
        computed: {
            current() {
                return this.$store.state.current;
            }
        }
    }
</script>

<style scoped>
    .three {
        background: greenyellow;
        /*height: 700px;*/
    }
    .box {
        width: 80%;
        margin: 0 auto 20px;
        border-radius: 10px;
        background: #f0f0f0;
        height: 200px;
        text-align: center;

    }
</style>
