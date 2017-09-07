<template>
  <div>
    <header>
<<<<<<< HEAD
      <h1>才豹学院单词默写报告</h1>
      <div class="back" v-if="view === 'exam' || view === 'report'">
        <el-button type="success" :disabled="isSubmit" @click="goBack()">返回</el-button>
      </div>
      <div class="back" v-else-if="view === 'pdf' && !isLecture">
=======
      <h1 v-if="userType==='teacher'">才豹学院单词默写报告</h1>
      <div class="back" v-if="view === 'exam' || view === 'report'">
        <el-button type="success" :disabled="isSubmit" @click="goBack()">返回</el-button>
      </div>
      <div class="back" v-else-if="view === 'pdf' && !isLecture && userType==='teacher'">
>>>>>>> 4cca2b60dfbf88ad5725160064cfe46f0acc6911
        <el-button type="success" :disabled="isSubmit" @click="lecture()">已讲解</el-button>
      </div>
    </header>
    <div v-bind:class="[judgeClass() ,'main']" style="">
<<<<<<< HEAD
=======
      <div v-if="view!=='pdf' && userType==='supervisor'">
        <span v-if="saveTime">保存时间：{{saveTime | dateall}}</span>
        <span v-if="saver">保存人：{{saver}}</span>
      </div>
>>>>>>> 4cca2b60dfbf88ad5725160064cfe46f0acc6911
      <div style="">
        <div class="tabel-head" style="">
          <div class="options" style="">
            <ul>
              <li class="first-li">
                <span class="li-head">编号</span>
                <span class="li-content copy-btn-pdf">{{No}}</span>
              </li>
<<<<<<< HEAD
              <li>
                <span class="li-head">学生</span>
                <span class="li-content copy-btn-pdf">{{studentNo}} {{studentName}}</span>
              </li>
              <li>
=======
              <li v-if="userType==='teacher'">
                <span class="li-head">学生</span>
                <span class="li-content copy-btn-pdf">{{studentNo}} {{studentName}}</span>
              </li>
              <li v-else-if="userType==='supervisor'">
                <span class="li-head">才豹号</span>
                <span class="li-content copy-btn-pdf">{{studentNo}}</span>
              </li>
              <li v-if="userType==='teacher'">
>>>>>>> 4cca2b60dfbf88ad5725160064cfe46f0acc6911
                <span class="li-head">默写内容</span>
                <span v-bind:class="[judgeFont(), 'li-content', 'copy-btn-pdf']" v-if="examType">{{examContent}} {{examType | stemName}} {{examRange}}</span>
              </li>
              <li>
                <span class="li-head">答题日期</span>
                <span class="li-content copy-btn-pdf" v-if="completionTime">{{completionTime | dateall}}</span>
              </li>
              <li>
                <span class="li-head">答题用时</span>
                <span class="li-content copy-btn-pdf">{{totalTime | minuteTime}}</span>
              </li>
            </ul>
          </div>
          <div class="total" style="">
            <span class="total-name">总分</span>
            <span class="total-Score">
              <span v-if="view === 'pdf'" class="copy-btn-pdf">{{totalScore}}</span>
              <input v-else v-model="totalScore" type="text" name="" :readonly="disabled">
            </span>
          </div>
        </div>
        <el-table :data="testPackageList" border id="list" style="width:100%;height:100%;padding:0;" :row-class-name="tableRowClassName">
          <el-table-column prop="" label="题号" align="center" width="40" class-name="copy-btn-pdf">
            <template scope="scope">
              <span>
                {{scope.$index * 1 + 1}}
              </span>
            </template>
          </el-table-column>
          <el-table-column prop="" label="反应时间(秒)" align="center" width="60" class-name="time copy-btn-pdf">
            <template scope="scope">
              <el-tooltip placement="top">
                <div slot="content">
                  <span v-for="(item, $index) in scope.row.InteractionList" :key="$index">
                    {{item.Action}}&nbsp;{{item.Time | filtNumber}}
                    <span v-if="item.CurrentAnswer">
                      {{item.CurrentAnswer}}
                    </span>
                    <br/>
                  </span>
                </div>
                <span>
                  {{scope.row.Time1 | filtNumber}}
                  <span v-if="scope.row.IsTimeout">(超时)</span>
                </span>
              </el-tooltip>
            </template>
          </el-table-column>
          <el-table-column prop="" label="题目" align="" class-name="copy-btn-pdf">
            <template scope="scope">
              <span class="copy-btn-pdf">
                {{scope.row.S.S}}
              </span>
            </template>
          </el-table-column>
          <el-table-column prop="WrittenAnswer" label="学生答案" align="" class-name="answer copy-btn-pdf">
            <template scope="scope">
              <span>
                {{scope.row.WrittenAnswer}}
                <span v-if="scope.row.IsForget">我不会</span>
              </span>
            </template>
          </el-table-column>
          <el-table-column prop="" label="正确答案" align="" class-name="copy-btn-pdf">
            <template scope="scope">
              <span>
                {{scope.row.A}}
              </span>
            </template>
          </el-table-column>
          <el-table-column prop="Score" label="得分" align="center" width="50" :class-name="setScoreCopyClass()">
            <template scope="scope">
              <span v-if="view === 'pdf'" class="copy-btn-pdf">{{scope.row.Score}}</span>
              <el-input v-else type="text" v-model="scope.row.Score" :readonly="disabled" @change="computedScore" style="text-align:center;"></el-input>
            </template>
          </el-table-column>
          <el-table-column prop="" label="错误情况分析" align="left" :class-name="setCopyClass()">
            <template scope="scope">
              <span v-if="view === 'pdf'" style="word-break: break-word;">{{scope.row.Comment}}</span>
              <el-input v-else v-model="scope.row.Comment" :readonly="disabled" type="textarea" autosize></el-input>
            </template>
          </el-table-column>
        </el-table>
        <div class="allComment" v-if="view !== 'pdf'">
          <p class="allComment-title">总评语</p>
          <el-input v-model="OverallComment" :readonly="disabled" type="textarea" :autosize="{ minRows: 4}"></el-input>
        </div>
      </div>
      <div class="submit-area" v-if="view === 'exam'">
<<<<<<< HEAD
        <el-button type="primary" :disabled="isSubmit" @click="updateData()">朕已阅</el-button>
=======
        <el-button type="success" :disabled="isSubmit" @click="saveData()">保存</el-button>
        <el-button type="danger" :disabled="isSubmit" @click="updateData()">朕已阅</el-button>
>>>>>>> 4cca2b60dfbf88ad5725160064cfe46f0acc6911
      </div>
    </div>
  </div>
</template>
<script type="application/ecmascript">
import teacherObj from '../../api/teacher'
import { mapGetters } from 'vuex'
import VueCookie from 'vue-cookie'
import Clipboard from 'clipboard'

export default {
  name: 'student',
  components: {
  },
  data() {
    return {
      msg: '',
<<<<<<< HEAD
=======
      userType: '',
>>>>>>> 4cca2b60dfbf88ad5725160064cfe46f0acc6911
      reportData: '',
      testPackageList: [],
      studentName: '',
      studentNo: '',
      examContent: '',
      userName: '',
      isLecture: false,
      No: '',
      examType: '',
      examRange: '',
      completionTime: '',
      startTime: '',
      range: {},
      totalTime: '',
      totalScore: '',
      CheckerState: '',
      OverallComment: '',
      postData:{
        token: '',
        TAId: '',
        IsSaving: '',
        CheckingResultPackage: {
          OverallComment: '',
          PackageItemList: []
        }
      },
      isSubmit: false,
      view: '',
      disabled: false,
      menuTab: '',
      finishTab: '',
      taid: ''
    }
  },
  computed: {
  ...mapGetters({
      token: 'token'
    })
  },
  created() {
    this.view = this.$route.query.view
<<<<<<< HEAD
=======
    this.userType = this.$route.query.userType
>>>>>>> 4cca2b60dfbf88ad5725160064cfe46f0acc6911
    document.onkeyup = this.keyUp
  },
  mounted() {
    this.taid = this.$route.query.taid
    this.menuTab = this.$route.query.menuTab
    this.finishTab = this.$route.query.finishTab
    if (this.view === 'exam') {
      this.getDatas(this.taid)
    } else {
      this.getReport(this.taid)
    }
    this.judgeView()
    this.clipboard()
  },
  methods: {
    getDatas (taid) {
      let that = this
      teacherObj.checkExam(taid).then(function(result) {
        if (result.data.code === 2) {
          that.setDatas(result)
          console.log(that.reportData)
        }
      })
    },
    getReport (taid) {
      let that = this
      teacherObj.getReport(taid).then(function(result) {
        if (result.data.code === 2) {
          that.setReportDatas(result)
        }
      })
    },
    setReportDatas (result) { // 报告字段
      let checkPackage = result.data.data.TestReport
      this.testPackageList = checkPackage.TestResultList.slice()
      this.studentName = checkPackage.TestAssignment.Student.Name
      this.studentNo = checkPackage.TestAssignment.Student.No
      this.No = checkPackage.TestAssignment.No
      this.CheckerState = checkPackage.TestAssignment.CheckerState
      this.examContent = checkPackage.TestAssignment.ObjectSet.Name
      this.isLecture = checkPackage.TestAssignment.LecturerState === 'Done'
      this.examRange = checkPackage.TestAssignment.AssignmentInfo.Range.Start + '-' + checkPackage.TestAssignment.AssignmentInfo.Range.End
      this.range = checkPackage.TestAssignment.AssignmentInfo.Range
      this.examType = checkPackage.TestAssignment.StrategyInfo.StemStrategy.Code
      this.completionTime = checkPackage.TestAssignment.CompletionInfo.CompletionTimestamp
      this.startTime = checkPackage.TestAssignment.AssignmentInfo.ExecutionTimeInterval.StartTimestamp
      this.totalTime = checkPackage.TestAssignment.OverallTestResult.TotalTime
      this.totalScore = checkPackage.TestAssignment.OverallTestResult.Correctness
      this.OverallComment = checkPackage.TestAssignment.OverallComment
      this.resetReportDatas(this.testPackageList)
      this.setTitle()
    },
    setDatas (result) { // 批阅字段数据格式化
      let checkPackage = result.data.data.CheckingPackage
      this.testPackageList = checkPackage.CheckingTaskList.slice()
      this.studentName = checkPackage.TestAssignment.Student.Name
      this.studentNo = checkPackage.TestAssignment.Student.No
      this.No = checkPackage.TestAssignment.No
      this.CheckerState = checkPackage.TestAssignment.CheckerState
      this.examContent = checkPackage.TestAssignment.ObjectSet.Name
      this.examRange = checkPackage.TestAssignment.AssignmentInfo.Range.Start + '-' + checkPackage.TestAssignment.AssignmentInfo.Range.End
      this.range = checkPackage.TestAssignment.AssignmentInfo.Range
      this.examType = checkPackage.TestAssignment.StrategyInfo.StemStrategy.Code
      this.completionTime = checkPackage.TestAssignment.CompletionInfo.CompletionTimestamp
      this.startTime = checkPackage.TestAssignment.AssignmentInfo.ExecutionTimeInterval.StartTimestamp
      this.totalTime = checkPackage.TestAssignment.OverallTestResult.TotalTime
      this.resetDatas(this.testPackageList)
      this.setTitle()
    },
    resetDatas (data) { // 定义输入字段
      console.log(data)
      if (this.CheckerState === 'Doing') {
        this.computedScore()
        this.testPackageList = this.sortList(data)
        return false
      }
      for (let i in data) {
        if (data[i]['WrittenAnswer']) {
          data[i]['Score'] = 2
        } else {
          data[i]['Score'] = 0
        }
        data[i]['Comment'] = ''
        if ((this.examType === 'S_C') && (data[i].WrittenAnswer === data[i].A)) {
          data[i].Score = 2
        } else if ((this.examType === 'S_C') && (data[i].WrittenAnswer !== data[i].A)) {
          data[i].Score = 0
        }
      }
      this.computedScore()
      this.testPackageList = this.sortList(data)
    },
    sortList (arr) {
      let arrWrong = []
      let arrRight = []
      let arrComment = []
      let arrNoComent = []
      for (let i in arr) {
        if (arr[i].Score * 1 === 2) {
          arrRight.push(arr[i])
        } else if (arr[i].Score * 1 === 0) {
          arrWrong.unshift(arr[i])
        } else {
          arrWrong.push(arr[i])
        }
      }
      for (let i in arrRight) {
        if (!arrRight[i].Comment) {
          arrNoComent.push(arrRight[i])
        } else {
          arrComment.push(arrRight[i])
        }
      }
      for (let i = 0; i < arrNoComent.length - 1; i ++) {
        for (let j = i + 1; j < arrNoComent.length; j ++) {
          if (arrNoComent[i].Time1 < arrNoComent[j].Time1) {
            let temp = arrNoComent[i]
            arrNoComent[i] = arrNoComent[j]
            arrNoComent[j] = temp
          }
        }
      }
      arrRight = arrComment.concat(arrNoComent)
      arr = arrWrong.concat(arrRight)
      return arr
    },
    resetReportDatas (data) {
      this.testPackageList = this.sortList(data)
    },
    computedScore () { // 总分计算
      let data = this.testPackageList
      let total = 0
      let all = data.length
      for (let i in data) {
        total += (data[i].Score * 1)
      }
      this.totalScore = Math.round((total / all) * 50)
    },
<<<<<<< HEAD
    updateData () {
      this.isSubmit = true
=======
    saveData () {
      this.isSubmit = true
      this.postData.IsSaving = true
      let isDatasetOver = this.setPostData(this.testPackageList)
      if (isDatasetOver === false) {
        return false
      }
      console.log(this.postData)
      this.submitData(this.postData)
    },
    updateData () {
      this.isSubmit = true
      this.postData.IsSaving = false
>>>>>>> 4cca2b60dfbf88ad5725160064cfe46f0acc6911
      let isDatasetOver = this.setPostData(this.testPackageList)
      if (isDatasetOver === false) {
        return false
      }
      console.log(this.postData)
      this.submitData(this.postData)
    },
    gotoPdf () {
      let url = location.href
      let pdfUrl = url.replace(/view=exam/g,'view=pdf')
      window.open(pdfUrl)
    },
    submitData (data) {
      let that = this
      teacherObj.postCheck(data).then(function (result) {
        if (result.data.code === 2) {
          that.isSubmit = false
          that.$router.push({
            path: '/class',
            query: {
              menuTab: that.menuTab,
              finishTab: that.finishTab
            }
          })
          that.gotoPdf()
<<<<<<< HEAD
=======
        } else if (result.data.code === 1 && that.postData.IsSaving) {
          that.isSubmit = false
          let number = result.data.data.OtherCheckerDoingTestAssignmentList[0].No
          that.$alert('当前还有编号为' + number + '的试卷正在批阅中，请继续完成批阅！')
          that.postData.CheckingResultPackage.PackageItemList = []
>>>>>>> 4cca2b60dfbf88ad5725160064cfe46f0acc6911
        } else {
          that.isSubmit = false
          that.postData.CheckingResultPackage.PackageItemList = []
        }
      })
    },
    judgeFont () {
      if (this.view === 'pdf') {
        return 'small-size'
      }
    },
    setPostData (data) {
      this.postData.token = VueCookie.get('teacherToken')
      this.postData.TAId = this.taid
<<<<<<< HEAD
      this.postData.IsSaving = false
=======
>>>>>>> 4cca2b60dfbf88ad5725160064cfe46f0acc6911
      this.postData.CheckingResultPackage.OverallComment = this.OverallComment
      for (let i in data) {
        let obj = {}
        obj['OId'] = data[i].OId
        if (data[i].Score === '' || data[i].Score * 1 > 2 || data[i].Score * 1 < 0) {
          this.$alert('第' + (i * 1 + 1) + '行分数输入有误!')
          this.postData.CheckingResultPackage.PackageItemList = []
          this.isSubmit = false
          return false
        }
        obj['Score'] = data[i].Score * 1
        obj['Comment'] = data[i].Comment
        this.postData.CheckingResultPackage.PackageItemList.push(obj)
      }
    },
    judgeClass () { // 判断当前入口，返回不同样式class
      if (this.view === 'exam' || this.view === 'report') {
        return 'main-wide'
      } else if (this.view === 'pdf') {
        return 'main-narrow'
      }
    },
    judgeView () {
      if (this.view === 'report') {
        this.disabled = true
      }
    },
    goBack () {
      let that = this
      let page = that.$route.query.page
      this.$router.push({
        path: '/class',
        query: {
          menuTab: that.menuTab,
          finishTab: that.finishTab,
          page: page
        }
      })
    },
    setTitle () { // 原生js设置标题
      let examContent, type, startTimeStr, startDate
      if (this.examContent) {
        examContent = (this.examContent.split('.')[2])
      }
      startTimeStr = new Date(this.startTime)
      startDate = (startTimeStr.getMonth() * 1 + 1) + '月' + startTimeStr.getDate() + '日'
      if (this.examType === 'S_C') {
        type = '中译英'
      } else if (this.examType === 'S_WF') {
        type = '英译中'
      }
      let part = this.levelToPart(this.range)
<<<<<<< HEAD
      if (this.view === 'pdf') {
=======
      if (this.view === 'pdf' && this.userType === 'supervisor') {
>>>>>>> 4cca2b60dfbf88ad5725160064cfe46f0acc6911
        window.document.title = this.studentName + startDate + examContent + 'P' + part + type + '-' + this.totalScore + '分'
      } else if (this.view === 'exam') {
        window.document.title = '编号：' + this.No
      }
    },
    judgeBg (item) {
      if (item.Score * 1 < 2 || item.Comment) {
        return 'yellow'
      }
    },
    judgeIsOver (time) {
      if (time > 30) {
        return 'yellow'
      }
    },
    tableRowClassName (row) {
      if (row.IsForget) {
        return 'write-nopadding'
      } else if ((row.Score * 1 < 2 || row.Comment) && this.view === 'pdf') {
        return 'write-yellow-score'
      } else if (row.IsTimeout || row.IsHesitant) {
        return 'write-yellow-time'
      } else if (row.Time2 * 1 > 15) {
        return 'write-yellow-time2'
      }
    },
    tableTdClassName (row) {
      if (row.IsForget) {
        return 'td'
      }
    },
    lecture () {
      let that = this
      let obj = {}
      that.isSubmit = true
      obj['TAId'] = that.taid
      obj['token'] = VueCookie.get('teacherToken')
      teacherObj.postLecture(obj).then(function(result) {
        that.isSubmit = false
        if (result.data.code === 2) {
          that.isLecture = true
        } else {
          $.alert('提交失败')
        }
      })
    },
    clipboard () { // 原生js复制到剪贴板
      let that = this
      let obj = new Clipboard('.copy-btn-pdf', {
        text: function (e) {
          var text = e.innerText.replace(/[\r\n]/g,"")
          that.$message({
            message: text + '已复制到剪贴板',
            duration: 1000
          })
          return text + '\n'
        }
      })
      return obj
    },
    setScoreCopyClass () {
      if (this.view === 'pdf') {
        return 'score copy-btn-pdf'
      } else {
        return 'score'
      }
    },
    setCopyClass () {
      if (this.view === 'pdf') {
        return 'copy-btn-pdf'
      } else {
        return ''
      }
    },
    levelToPart (item) {
      if (item.Start * 1 === 1 && item.End * 1 === 50) {
        return '1'
      } else if (item.Start * 1 === 51 && item.End * 1 === 100) {
        return '2'
      } else if (item.Start * 1 === 101 && item.End * 1 === 150) {
        return '3'
      } else if (item.Start * 1 === 151 && item.End * 1 === 200) {
        return '4'
      } else if (item.Start * 1 === 1 && item.End * 1 === 25) {
        return 'a'
      } else if (item.Start * 1 === 26 && item.End * 1 === 50) {
        return 'b'
      } else if (item.Start * 1 === 51 && item.End * 1 === 75) {
        return 'c'
      } else if (item.Start * 1 === 76 && item.End * 1 === 100) {
        return 'd'
      } else if (item.Start * 1 === 101 && item.End * 1 === 125) {
        return 'e'
      } else if (item.Start * 1 === 126 && item.End * 1 === 150) {
        return 'f'
      } else if (item.Start * 1 === 151 && item.End * 1 === 175) {
        return 'g'
      } else if (item.Start * 1 === 176 && item.End * 1 === 200) {
        return 'h'
      } else {
        return '(' + item.Start + '-' + item.End + ')'
      }
    },
    keyUp (e) { // 原生js上下左右移动焦点
      let currKey = 0
      let that = this
      let inputs = document.getElementById("list").getElementsByTagName("input")
      let textareas = document.getElementById("list").getElementsByTagName("textarea")
      let current
      let focus = document.activeElement
      e = e || event
      currKey = e.keyCode || e.which || e.charCode
      console.log(currKey)
      for(var i = 0; i < inputs.length; i++) {
        if (inputs[i] === focus) {
          current = 'input'
          break
        } else if (textareas[i] === focus) {
          current = 'textarea'
          break
        }
      }
      switch(currKey)
       {
        case 38:
          if (i - 1 >= 0 && current === 'input') {
            inputs[i - 1].focus()
            inputs[i - 1].select()
          } else if (i - 1 >= 0 && current === 'textarea') {
            textareas[i - 1].focus()
          }
          break
        case 40:
          if (i + 1 < inputs.length  && current === 'input') {
            inputs[i + 1].focus()
            inputs[i + 1].select()
          } else if (i + 1 < inputs.length  && current === 'textarea') {
            textareas[i + 1].focus()
          }
          break
       } 
    }
  }
}
</script>
<style scoped>
  header {
    padding: 20px 0;
  }
  h1 { 
    text-align: center;
  }
  .el-input input{
    border: none;
  }
  .main {
    height:100%;
    margin:0 auto;
  }
  .main-wide {
    width: 60%;
  }
  .main-narrow {
    max-width: 700px;
  }
  .tabel-head {
    width:100%;
    height:100%;
    padding:0;
    display:flex;
  }
  .options {
    padding: 0;
    flex:6;
  }
  .options li {
    border: 1px solid #dfe6ec;
    border-top:none;
    line-height: 40px;
    overflow: hidden;
    display: flex;
  }
  .options li span {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  li.first-li {
    border-top: 1px solid #dfe6ec;
  }
  .li-head {
    width: 50%;
    text-align: center;
    border-right: 1px solid #dfe6ec;
    background-color: #eef1f6;
  }
  .li-content {
    background-color: #fff;
    line-height: 40px;
    width: 50%;
    text-align: center;
  }
  .total {
    flex:4;
    display: flex;
    border: 1px solid #dfe6ec;
    border-left: none;
  }
  .total-name {
    width: 50%;
    text-align: center;
    background-color: #eef1f6;
    border-right: 1px solid #dfe6ec;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .total-Score {
    width: 50%;
    background-color: #fff;
    font-size: 64px;
    color: red;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .total-Score input {
    border: none;
    width: 100%;
    height: 100%;
    text-align: center;
    font-size: 64px;
    color: red;
  }
  .submit-area {
    background-color: #fff;
    text-align: center;
    padding: 30px 0;
  }
  .allComment {
    padding:20px 10px;
    background-color: #fff;
  }
  .allComment-title {
    text-align: left;
  }
  .small-size {
    font-size: 13px;
  }
  .back{
    position: absolute;
    right: 10%;
    top:20px;
  }
  .yellow {
    background-color: yellow;
    display: inline-block;
    width: 100%;
  }
</style>
<style>
  .el-table tr.write-nopadding .answer{
    padding: 0;
    background-color: grey;
    color: #fff;
  }
  .el-table tr.write-yellow-time .time{
    padding: 0;
    background-color: yellow;
  }
  .el-table tr.write-yellow-time2 .time2{
    padding: 0;
    background-color: yellow;
  }
  .el-table tr.write-yellow-score .score{
    padding: 0;
    background-color: yellow;
  }
  .el-table .cell {
    word-break: break-word;
    padding: 5px;
  }
</style>
