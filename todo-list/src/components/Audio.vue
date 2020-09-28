<template>
  <base-container>
    <div>Телефония</div>
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-3 col-sm-6">
          <div class="fillters-telephony ">
            <div class="date-picker">
              <date-range-picker class="date-picker" @change="changeDateRange"></date-range-picker>
            </div>
            <div class="phone-number">
              <input @change="updateData" v-model="filters.phone" type="number" placeholder="Номер телефона" class="form-control input__phone">
            </div>
            <div class="direction">
              <select @change="updateData" v-model="filters.direction" class="form-control">
                <option value="all">Направление</option>
                <option value="1">Входящий</option>
                <option value="2">Исходящий</option>
              </select>
            </div>
          </div><!-- /.fillters-telephony -->
        </div><!-- /.col-lg-4.col-sm-6 -->
      </div><!-- /.row -->
      <div class="container-fluid  bg-blocks mt-4">
        <b-table
          :fields="tableCalls.fields"
          :items="tableCalls.items"
          show-empty
          :responsive="true"
        >
          <template v-slot:empty>
            <div class="sk-wave">
              <div class="sk-rect sk-rect-5"></div>
              <div class="sk-rect sk-rect-4"></div>
              <div class="sk-rect sk-rect-3"></div>
              <div class="sk-rect sk-rect-2"></div>
              <div class="sk-rect sk-rect-1"></div>
            </div>
          </template>
          <template v-slot:cell(recordName)="data">
            <audio v-if="data.value" controls>
              <source :src="data.value" type="audio/wav">
            </audio>
            <span v-else>Нет</span>
          </template>
        </b-table>
        <b-pagination
          v-model="tableCalls.page"
          :total-rows="tableCalls.totalCount"
          :per-page="tableCalls.perPage"
          @change="changePage"
        ></b-pagination>
      </div><!-- /.container-fluid.bg-blocks.mt-4 -->
    </div>
  </base-container>
</template>

<script>
import BaseContainer from '@/components/BaseContainer.vue'
import DateRangePicker from '@/components/DateRangePicker.vue'
export default {
  name: 'HomeContainer',
  components: {
    DateRangePicker,
    'base-container': BaseContainer
  },
  data: function () {
    return {
      filters: {
        from: window.moment().subtract(30, 'day').format('YYYY-MM-DD'),
        to: window.moment().format('YYYY-MM-DD'),
        direction: 'all',
        phone: ''
      },
      tableCalls: {
        fields: [
          {
            key: 'id',
            label: '№'
          },
          {
            key: 'employeeId',
            label: '№ резюме'
          },
          {
            key: 'name',
            label: 'Рекрутер'
          },
          {
            key: 'phoneNumber',
            label: 'Телефон'
          },
          {
            key: 'direction',
            label: 'Направление'
          },
          {
            key: 'callStart',
            label: 'Дата'
          },
          {
            key: 'duration',
            label: 'Продолжительность'
          },
          {
            key: 'recordName',
            label: 'Аудиозапись'
          }
        ],
        items: [],
        page: 1,
        totalCount: 0,
        perPage: 20
      }
    }
  },
  created () {
    this.updateData()
  },
  methods: {
    changeDateRange (event) {
      this.filters.from = event.from
      this.filters.to = event.to
      this.updateData()
    },
    changePage (page) {
      this.tableCalls.page = page
      this.updateData()
    },
    updateData () {
      const formData = new FormData()
      for (const key in this.filters) {
        formData.append('filters[' + key + ']', this.filters[key])
      }
      formData.append('page', this.tableCalls.page)
      window.axios4api.post('/get_calls', formData)
        .then(response => {
          if (response.data) {
            response.data.rows.forEach(el => {
              for (const key in el) {
                switch (key) {
                case 'direction':
                  el[key] = (el.direction === 1) ? 'Входящий' : 'Исходящий'
                  break
                case 'callStart':
                  el[key] = window.moment(el.callStart.date).format('DD-MM-YYYY')
                  break
                }
              }
            })
            this.tableCalls.items = response.data.rows
            this.tableCalls.totalCount = response.data.totalCount
          }
        })
    }
  }
}
</script>
