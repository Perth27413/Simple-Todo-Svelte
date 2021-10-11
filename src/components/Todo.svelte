<script lang="ts">
  import { onMount } from 'svelte';
  import dayjs from 'dayjs'
  import DateModel from '../model/DateModel'
  import TaskModel from '../model/TaskModel'

  let date: DateModel = new DateModel
  let inputTask: TaskModel = new TaskModel
  let taskLists: Array<TaskModel> = new Array<TaskModel>()

  onMount(() => {
    mapDataToDate()
  })

  function mapDataToDate(): void {
    date.day = getDayName(dayjs().day())
    date.date = String(dayjs().date())
    date.month = getMonthName(dayjs().month())
    date.year = String(dayjs().year())
  }

  function getDayName(dayIndex: number): string {
    const dayName: Array<string> = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
    return dayName[dayIndex]
  }

  function getMonthName(monthIndex: number): string {
    const monthName: Array<string> = ["JAN", "FEB", "MAR", "APR", "MAY", "JUN", "JUL", "AUG", "SEP", "OCT", "NOV", "DEC"]
    return monthName[monthIndex]
  }

  function onKeyPress(e: any): void {
    if (e.charCode === 13) {
      taskLists = [...taskLists, inputTask]
      inputTask = new TaskModel
    }
  }

  function deleteTask(index: number): void {
    let tasks: Array<TaskModel> = [...taskLists]
    tasks.splice(index, 1)
    taskLists = tasks
  }
</script>

<main>
  <div id="todo">
    <div id="todoBox">
      <div id="headerBox">
        <div id="dateBox">
          <div id="date">
            {date.date}
          </div>
          <div id="monthYear">
            <div id="month">{date.month}</div>
            <div id="year">{date.year}</div>
          </div>
        </div>
        <div id="dayBox">{date.day}</div>
      </div>
      <div id="addTaskBox">
        <i id="inputIcon" class="fas fa-list"></i>
        <input type="text" bind:value="{inputTask.taskName}" id="inputTask" placeholder="Add a task..." on:keypress="{onKeyPress}">
      </div>
      <div id="listBox">
        {#each taskLists as task, index}
          <div class="task">
            <input type="checkbox" class="input-check-box" bind:checked="{task.isDone}">
            <div class="task-name {task.isDone ? 'disable' : ''}">{task.taskName}</div>
            <div class="task-icon" on:click="{() => deleteTask(index)}"><i id="taskIcon" class="fas fa-trash"></i></div>
          </div>
        {/each}
      </div>
      <div id="footer">
        <hr>
        <div id="footerText">Svelte 101</div>
      </div>
    </div>
  </div>
</main>

<style lang="scss">
  #todo {
    @mixin scrollCustom() {
      &::-webkit-scrollbar {
        height: 20px;
        width: 20px;
        background-color: rgba(255, 255, 255, 0);
      }
      &::-webkit-scrollbar-track,
      &::-webkit-scrollbar-thumb {
        border: 6px solid rgba(255, 255, 255, 0);
        background-clip: padding-box;
      }
      &::-webkit-scrollbar-track {
        background-color: #e2e2e2;
        border-radius: 20px;
      }
      &::-webkit-scrollbar-thumb {
        background-color: rgb(193, 193, 193);
        border-radius: 20px;
        &:hover {
          background-color: #8b8b8b;
          cursor: pointer;
        }
      }
    }
    width: 100%;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    #todoBox {
      width: 350px;
      height: 550px;
      background-color: #fff;
      border-radius: 15px;
      padding: 20px 20px 10px 20px;
      box-shadow: 3px 2px 8px #888888;
      #headerBox {
        display: flex;
        height: 50px;
        margin-bottom: 15px;
        #dateBox {
          color: #767676;
          display: flex;
          width: 50%;
          #date {
            font-size: 52px;
            font-weight: bold;
            line-height: 45px;
          }
          #monthYear {
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            font-size: 18px;
            font-weight: bold;
          }
        }
        #dayBox {
          width: 50%;
          display: flex;
          justify-content: flex-end;
          align-items: center;
          color: #00B2FF;
          font-weight: bold;
          font-size: 21px;
        }
      }
      #addTaskBox {
        width: 100%;
        position: relative;
        margin-bottom: 20px;
        #inputTask {
          width: 100%;
          height: 35px;
          border-radius: 5px;
          border-color: #0085FF;
          outline: none;
          padding: 0 10px 0 40px;
        }
        #inputIcon {
          position: absolute;
          top: 50%;
          transform: translateY(-50%);
          margin-left: 10px;
        }
      }
      #listBox {
        height: calc(100% - 171px);
        margin-bottom: 20px;
        overflow-y: overlay;
        @include scrollCustom();
        .task {
          $check-box-size: 20px;
          display: flex;
          margin-bottom: 10px;
          .input-check-box {
            width: $check-box-size;
            height: $check-box-size;
            cursor: pointer;
          }
          .task-name {
            width: 90%;
            padding-left: 15px;
            font-weight: bold;
            color: #767272;
            display: flex;
            align-items: center;
            &.disable {
              color: #D6D6D6;
            }
          }
          .task-icon {
            width: calc(10% - #{$check-box-size});
            display: flex;
            justify-content: flex-end;
            align-items: center;
            color: #ADADAD;
            margin-right: 20px;
            &:hover {
                color: rgb(61, 61, 61);
                cursor: pointer;
              }
            &.disable {
                color: #E0E0E0;
                cursor: default;
              }
          }
        }
      }
      #footer {
        #footerText {
          width: 100%;
          text-align: center;
          font-weight: bold;
          color: #BCBCBC;
        }
      }
    }
  }
</style>