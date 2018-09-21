<template>
  <div class="category-view subview">
     <div class="layout">
        <div class="ucenter-header">
            <div class="avatar"></div>
            <div class="nologin" v-if='flag'>
            <a class="btn-login" @click='gopage("/login")'>点击登录</a>
            </div>
            <div class="logining" v-else>
                <span class="username">{{username}}</span>
                <a class="btn-logout" @click='logout'>注销</a>
            </div>
        </div>
        <div class="ucenter-content">
            <h3>我的收藏</h3>
            <div>
               <ul class="th-tabbar">
                  <li class="th-tabbar-item" @click= "tabsSwitch(index)"  v-for= "(tab,index) in tabsName" :key= 'index' v-bind:class= "{active:tab.isActive}">{{tab.name}}</li>
               </ul>
            </div>
            <div class="tab-content">
                <div class="th-tabpannel">
                    <div class="th-tabpannel-wrap">
                      <!--歌曲-->
                      <div class="th-tabpanel-item">
                          <div class="tab-pane ucenter-songs" v-bind:class="{active1:activeA}">
                             <div class="playall">
                                <span class="iconfont">&#xe65c;</span>
                                播放全部
                                <i>( 2首 )</i>
                             </div>
                             <div>
                                <ul class="th-songlist">
                                  <li class="th-songlist-item">
                                      <div class="left">
                                          <div class="info">
                                              <div class="line1">
                                              <span class="name">青春不一样</span>
                                              </div>
                                              <span class="author">金志文</span>
                                          </div>
                                      </div>
                                      <div class="iconfont icon-favor icon-favor1"></div>
                                      <div class="download iconfont icon-download"></div>
                                  </li>
                                </ul>
                             </div>
                          </div>
                          <div class="tab-pane ucenter-songs" v-bind:class="{active2:activeB}">
                               <div class="no-login">
                               <p>请登陆后查看收藏</p>
                               </div>
                          </div>
                      </div>
                       <!--专辑-->
                      <div class="th-tabpanel-item" style="display: none;">
                          <div class="tab-pane ucenter-albums" v-bind:class="{active1:activeA}">
                             <div>
                                <ul class="th-list album">
                                  <li class="url arrow-right log">
                                      <div class="pic">
                                          <img src="http://qukufile2.qianqian.com/data2/pic/49e6161afb13e3eda9d1cb4e304561a2/584551506/584551506.jpg@s_1,w_90,h_90" />
                                      </div>
                                      <div class="info">
                                          <span class="name">青春不一样</span>
                                          <span class="author">金志文</span>
                                     </div>
                                      <div class=" iconfont icon-arrow-right">&#xe60f;</div>
                                  </li>
                                </ul>
                             </div>
                          </div>
                          <div class="tab-pane ucenter-albums" v-bind:class="{active2 : activeB}">
                               <div class="no-login">
                               <p>请登陆后查看收藏</p>
                               </div>
                          </div>
                      </div>
                       <!--歌单-->
                       <div class="th-tabpanel-item" style="display: none;">
                          <div class="tab-pane ucenter-songlists" v-bind:class="{active1:activeA}">
                             <div>
                                <ul class="th-list album">
                                  <li class="url arrow-right log">
                                      <div class="pic">
                                          <img src="http://qukufile2.qianqian.com/data2/pic/49e6161afb13e3eda9d1cb4e304561a2/584551506/584551506.jpg@s_1,w_90,h_90" />
                                      </div>
                                      <div class="info">
                                          <span class="name">把心事 ♬装进耳机里</span>
                                          <span class="author">共15首&nbsp;&nbsp;&nbsp;&nbsp;by 傲娇娇</span>
                                     </div>
                                      <div class=" iconfont icon-arrow-right">&#xe60f;</div>
                                  </li>
                                </ul>
                             </div>
                          </div>
                          <div class="tab-pane ucenter-songlists" v-bind:class="{active2:activeB}">
                               <div class="no-login">
                               <p>请登陆后查看收藏</p>
                               </div>
                          </div>
                      </div>
                       <!--歌手-->
                       <div class="th-tabpanel-item" style="display: none;">
                          <div class="tab-pane ucenter-artists" v-bind:class="{active1:activeA}">
                             <div>
                                <ul class="row">
                                  <li class="artist arrow-right log">
                                      <div class="pic">
                                          <img src="http://qukufile2.qianqian.com/data2/pic/140d09665d1c204efe00973c3e16282c/579342600/579342600.jpg@s_0,w_120" />
                                      </div>
                                      <div class="author">薛之谦</div>
                                  </li>
                                </ul>
                             </div>
                          </div>
                          <div class="tab-pane ucenter-artists" v-bind:class="{active2:activeB}">
                               <div class="no-login">
                               <p>请登陆后查看收藏</p>
                               </div>
                          </div>
                      </div>
                   </div>
                </div>
            </div>
        </div>
     </div>
  </div>
</template>
<script>
import '../../assets/font1/iconfont.css'
export default {
  data () {
    return {
      tabsName: [
        {
          name: '歌曲',
          isActive: true
        },
        {
          name: '专辑',
          isActive: false
        },
        {
          name: '歌单',
          isActive: false
        },
        {
          name: '歌手',
          isActive: false
        }
      ],
      active: false,
      flag: true,
      username: '',
      activeA: false,
      activeB: true
    }
  },
  mounted () {
    if (localStorage.getItem('username')) {
      this.flag = false
      this.username = localStorage.getItem('username')
      this.activeB = false
      this.activeA = true
    } else {
      this.flag = true
      this.activeA = false
      this.activeB = true
    }
  },
  methods: {
    tabsSwitch: function (tabIndex) {
      var tabCardCollection = document.querySelectorAll('.th-tabpanel-item')
      const len = tabCardCollection.length
      for (var i = 0; i < len; i++) {
        tabCardCollection[i].style.display = 'none'
        this.tabsName[i].isActive = false
      }
      this.tabsName[tabIndex].isActive = true
      tabCardCollection[tabIndex].style.display = 'block'
    },
    gopage (path) {
      this.$router.push(path)
    },
    logout () {
      localStorage.removeItem('username')
      this.flag = true
      this.activeA = false
      this.activeB = true
    }
  }
}
</script>
<style lang="scss">
@import'../../lib/reset.scss';
.category-view{
    @include rect(100%, auto);
    .layout{
      position: fixed;
      width: 100%;
      top: 98px;
      bottom: 0;
      z-index: 100;
       .ucenter-header{
           @include rect(100%,0.85rem);
           @include background-color(#f8f8f8);
           @include padding(0 0.17rem);
           overflow:hidden;
           .avatar{
               @include rect(0.56rem, 0.56rem);
               background: url(../../assets/app-all-23f.png) no-repeat;
               background-position: -184px -144px;
               @include border-radius(0.28rem);
               @include margin(0.15rem 0.1rem 0 0);
               background-size: 289px;
               float:left;
            }
            .username{
                overflow: hidden;
                text-overflow: ellipsis;
                white-space: nowrap;
                max-width: 120px;
                float: left;
                display:block;
                @include line-height(0.85rem);
            }
            .btn-login{
              float:left;
              @include text-color(#555);
              @include line-height(0.85rem);
              @include font-size(.16rem);
            }
            .btn-logout{
              float:right;
              @include rect(0.5rem, 100%);
              @include text-color(#555);
              @include font-size(.16rem);
              display:block;
              @include line-height(0.85rem);
            }
       }
       .ucenter-content{
           @include padding(0.2rem 0.17rem 0);
           h3{
              @include font-size(.2rem);
           }
           .th-tabbar{
               @include flexbox();
               border-bottom: 1px solid #eaeaea;
               .active{
                  border-bottom: 1px solid #e13228;
               }
               .th-tabbar-item{
                 @include padding(0.18rem 0 0.12rem);
                 @include text-align(center);
                 @include font-size(.16rem);
                 @include flex();
                 @include text-color(#999);
               }
           }
          .tab-content{
            .th-tabpannel{
              .th-tabpannel-wrap{
                 @include flexbox();
                 .th-tabpanel-item{
                    @include rect(100%, auto);
                    .tab-pane{
                      display:none;
                      .playall{
                        .iconfont{
                          @include font-size(.24rem);
                          @include line-height(.6rem);
                          @include margin(0 0.05rem 0 0)
                        }
                        @include rect(100%, .6rem);
                        @include font-size(.18rem);
                        border-bottom: 1px solid #f2f2f2;
                        @include line-height(.6rem);
                        i{
                          @include text-color(#999);
                          @include font-size(0.13rem);
                        }
                      }
                      .no-login{
                        @include rect(100%, 230px);
                        @include text-align(center);
                        p{
                          @include margin(.8rem auto .2rem);
                          @include text-color(#999);
                        }
                      }
                      .th-songlist{
                        @include rect(100%, auto);
                        .th-songlist-item{
                           @include rect(100%, 0.55rem);
                           @include text-align(left);
                           @include padding(0.07rem 0);
                           border-bottom: 1px solid #f2f2f2;
                           @include flexbox();
                           box-sizing: border-box;
                          @include flex-direction(row);
                           .left{
                               @include flex();
                               @include align-items(center);
                               padding-left:0.1rem;
                               .line1{
                                  .name{
                                    @include font-size(.16rem);
                                  }
                               }
                               .author{
                                    @include font-size(.13rem);
                                    @include text-color(#999);
                                  }
                           }
                           .icon-favor{
                             @include text-color(#333);
                             @include rect(0.4rem, 0.48rem);
                             @include font-size(.24rem);
                             @include text-align(center);
                           }
                          .icon-favor:before{
                             content:"\e613";
                          }
                          .icon-favor1:before{
                             content:"\e603";
                          }
                          .download{
                            @include rect(0.36rem, 0.36rem);
                            @include font-size(.22rem);
                            @include text-align(center);
                            @include line-height(.36rem);
                          }
                        }
                      }
                      .th-list{
                        .arrow-right{
                          @include flexbox();
                          border-bottom: 1px solid #f2f2f2;
                          @include rect(100%, .76rem);
                          @include align-items();
                          .pic{
                            @include rect(.54rem, .54rem);
                            margin-right:15px;
                            img{
                              @include rect(100%, 100%);
                            }
                          }
                          .info{
                            @include flex();
                            @include align-items(center);
                            overflow: hidden;
                            text-overflow: ellipsis;
                            .name{
                             @include font-size(.16rem);
                             display:block;
                            }
                            .author{
                                    @include font-size(.13rem);
                                    @include text-color(#999);
                            }
                          }
                          .icon-arrow-right{
                            @include font-size(.22rem);
                          }
                        }
                      }
                    }
                    .active1{
                       display:block;
                     }
                     .active2{
                       display:block;
                     }
                    .ucenter-artists{
                      .row{
                        @include flexbox();
                        flex-wrap: wrap;
                        @include padding(.12rem 0);
                        .artist{
                          @include flex();
                          @include margin(.12rem 0);
                           max-width: 33.333%;
                           @include text-align();
                          .pic{
                            @include rect(.75rem, .75rem);
                            border: 1px solid #f1f1f1;
                            -webkit-border-radius: 37.5px;
                            border-radius: 37.5px;
                            margin:0 auto;
                            img{
                              @include rect(100%,100%);
                              border-radius: 50%;
                            }
                          }
                          .author{
                            @include rect(100%,auto);
                            @include margin(.1rem 0 0 0);
                            overflow: hidden;
                            text-overflow: ellipsis;
                            white-space: nowrap;
                          }
                        }
                      }
                    }
                 }
              }
            }
          }
       }
    }
}
</style>
