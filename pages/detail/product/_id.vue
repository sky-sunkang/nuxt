<template>
  <div>
    <V-HeadNev selectNev="product"/>
    <V-HeadBanner/>
    <div class="container">
      <div class="content-wapper ">
        <el-container>
            <V-Left :leftproducts="leftproducts"/>
          <el-main>
            <div>
              您所在的位置：网站首页 > 产品展示 > 产品详情
              <el-divider></el-divider>
              <div class="product-crove">
                <el-image :lazy='true' :src="'/publiccmswebfile/'+data.cover" width="400" :alt="data.product_name"></el-image>
              </div>
              <div class="product-into">
                <div class="product-title"><h4> {{data.title}}</h4></div>
                <img class="scale" src="~/assets/images/index/3Dprintweixin.jpg" width="60" height="60" />
                <p>价格信息：</p>
                <p>{{data.price}}</p>
                <p>产品品牌：{{data.author}}</p>
                <p>供应总量：{{data.product_total}}</p>
                <p style="color: red">价格可谈，量大长期合作者从优，详情请电话、微信联系或公司面谈</p>
              </div>
              <div class="product-detail" v-html="htmlDecode(data.text)">

              </div>
              <div class="prev_next">
                <a class="prev" v-if="data.prev_id" :href="'/detail/product/'+data.prev_id"><i class="el-icon-d-arrow-left"></i>上一篇：{{data.prev_title}}</a>
                <a class="next" v-if="data.next_id" :href="'/detail/product/'+data.next_id">下一篇：{{data.next_title}}<i class="el-icon-d-arrow-right"></i></a>
              </div>
            </div>
          </el-main>
        </el-container>
      </div>
    </div>
  </div>
</template>

<script>
  import Foot from '~/components/Foot'
  import HeadBanner from '~/components/HeadBanner'
  import HeadNev from '~/components/HeadNev'
  import Left from '~/components/Left'
  export default {
    name: 'new-detail',
    components: {
      'Foot': Foot,
      'V-HeadBanner': HeadBanner,
      'V-HeadNev': HeadNev,
      'V-Left':Left
    },
    data() {
      return {
      }
    },created(){
    },
    head(){
      return {
        title: this.data.product_name,
        meta: [
          { hid: 'keywords', name: 'keywords', content: this.data.keywords },
          { hid: 'description', name: 'description', content:  this.data.description  }
        ]
      }
    },
    methods: {
      htmlDecode (value) { // 把转义的字符串转义回来
        return !value ? value : String(value).replace(/&gt;/g, ">").replace(/&lt;/g, "<").replace(/&quot;/g, '"').replace(/&amp;/g, "&").replace(/&#39;/g, "\'").replace(/&nbsp;/g," ").replace(/http:/g,"").replace(/\/\/sk-yye.cn:8080\/publiccms\/webfile/g,"/publiccmswebfile");
      },
      //时间格式化函数，此处仅针对yyyy-MM-dd hh:mm:ss 的格式进行格式化
      dateFormat(time) {
        var date=new Date(time);
        var year=date.getFullYear();
        /* 在日期格式中，月份是从0开始的，因此要加0
         * 使用三元表达式在小于10的前面加0，以达到格式统一  如 09:11:05
         * */
        var month= date.getMonth()+1<10 ? "0"+(date.getMonth()+1) : date.getMonth()+1;
        var day=date.getDate()<10 ? "0"+date.getDate() : date.getDate();
        var hours=date.getHours()<10 ? "0"+date.getHours() : date.getHours();
        var minutes=date.getMinutes()<10 ? "0"+date.getMinutes() : date.getMinutes();
        var seconds=date.getSeconds()<10 ? "0"+date.getSeconds() : date.getSeconds();
        // 拼接
        return year+"-"+month+"-"+day+" "+hours+":"+minutes+":"+seconds;
      }
    },
    async asyncData ({ params, error ,$axios}) {
      try {
        const prores = await $axios.get('/api/directive/contentList?showParameters=false&categoryId=122&pageIndex=1&count=5')
        const res = await $axios.get('/member/content.json?id='+params.id)
        return {data:res.data,leftproducts:prores.data.page.list}
      } catch (e) {
        error({ message: 'User not found', statusCode: 404 })
      }
    }
  }
</script>

<style scoped>
  .content-wapper {
    width: 1200px;
    min-height: 500px;
    margin: 0 auto;
  }
  .container {
    margin-top: 20px;
  }
  .content-wapper  .el-divider--horizontal{
    margin: 10px 0;
  }
  .right-left a{
     color:rgb(0, 0, 238) ;
  }
  .right-left a:hover {
    color: #faaf5c;
  }
  .product-crove{
    width: 300px;
    height: 300px;
    float: left;
  }
  .product-crove .el-image{
    width: 300px;
    height: 300px;
  }
  .product-into{
    float: right;
    text-align: left;
    width: 650px;
    height: 300px;
  }
  .product-into p{
    margin: 0;
    line-height: 45px;
  }
  .product-title{
    font-size: 18px;
    font-weight: bold;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
  }
  .product-title h4{
    margin: 0;
  }
  .product-detail{
    clear: both;
    padding-top: 30px;
  }

  .scale:hover{
    transform: scale(3);
  }
  .prev_next{
    clear: both;
    margin-top: 20px;
  }
  .prev{
    width: 50%;
    padding: 10px;
    font-size: 14px;
    color: #666161;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    float: left;
    text-align: center;
    background-color: #F5F5F5;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .next{
    width: 49%;
    padding: 10px;
    font-size: 14px;
    color: #666161;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    float: right;
    text-align: center;
    background-color: #F5F5F5;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
</style>
