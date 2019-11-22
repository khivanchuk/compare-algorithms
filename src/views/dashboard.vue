<template>
  <div>
    <NavBar />
    <div class="container">
      <div class="tile is-ancestor">
        <div class="tile is-parent">
          <div class="tile is-child box">
            <p>
              This Demo illustrates the complexity of
              <span class="has-text-weight-semibold">Sorting Algorithms</span>
              for
              <span class="has-text-weight-semibold">Randomly Generated</span>
              Data,
              <span class="has-text-weight-semibold">Already Sorted</span>
              Data and
              <span class="has-text-weight-semibold">Reversed Data</span>. The
              <span class="has-text-weight-semibold">Insertion Sort</span>,
              <span class="has-text-weight-semibold">Selection Sort</span>,
              <span class="has-text-weight-semibold">Quick Sort</span>
              and native
              <span class="has-text-weight-semibold">Javascript Sort</span>
              algorithms are compared. Algorithms performance is measured in
              milliseconds.
            </p>
          </div>
        </div>
      </div>
      <div class="tile is-ancestor">
        <div class="tile is-parent is-4">
          <div class="tile is-child box">
            <DataEditor @onDataSelected="measurePerformance" />
          </div>
        </div>
        <div class="tile is-parent">
          <div class="tile is-child box">
            <h2 class="title is-5">Sorting Complexity Chart</h2>
            <line-chart
              :width="600"
              :height="300"
              :labels="this.displayedLabels"
              :datasets="this.displayedDatasets"
              :options="$options.options"
            ></line-chart>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NavBar from '@/components/navbar.vue';
import DataEditor from '@/components/dataEditor.vue';
import LineChart from '@/components/lineChart';
import { DATA_ORDER_OPTIONS, DATA_COUNT } from '@/shared/constants';

const options = {
  scales: {
    yAxes: [
      {
        ticks: {
          beginAtZero: true,
        },
      },
    ],
  },
};

export default {
  name: 'Dashboard',
  options,
  components: {
    NavBar,
    DataEditor,
    LineChart,
  },
  data() {
    return {
      insertionSortData: [],
      selectionSortData: [],
      quickSortData: [],
      nativeSortData: [],
    };
  },
  methods: {
    generateData(count) {
      return Array.from({ length: count }, () =>
        Math.floor(Math.random() * count)
      );
    },

    doNativeSort(data) {
      return data.sort((a, b) => a - b);
    },

    reverseData(data) {
      return data.sort((a, b) => b - a);
    },

    processData(count, order) {
      let generatedData = this.generateData(count);
      let processedData = [];

      switch (order) {
        case DATA_ORDER_OPTIONS.SORTED:
          processedData = this.doNativeSort(generatedData);
          break;
        case DATA_ORDER_OPTIONS.REVERSED:
          processedData = this.reverseData(generatedData);
          break;
        default:
          processedData = [...generatedData];
          break;
      }
      return processedData;
    },

    measurePerformance(order) {
      this.insertionSortData = [];
      this.selectionSortData = [];
      this.quickSortData = [];
      this.nativeSortData = [];

      DATA_COUNT.forEach(count => {
        const data = this.processData(count, order);
        let t0, t1;
        console.log(data);

        let dataToSort = [...data];
        t0 = performance.now();
        this.doInsertionSort(dataToSort);
        t1 = performance.now();
        this.insertionSortData.push(t1 - t0);

        console.log(dataToSort);
        console.log(`Call to doInsertionSort took ${t1 - t0} milliseconds.`);

        dataToSort = [...data];
        t0 = performance.now();
        this.doSelectionSort(dataToSort);
        t1 = performance.now();
        this.selectionSortData.push(t1 - t0);

        console.log(dataToSort);
        console.log(`Call to doSelectionSort took ${t1 - t0} milliseconds.`);

        dataToSort = [...data];
        t0 = performance.now();
        this.doQuickSort(dataToSort);
        t1 = performance.now();
        this.quickSortData.push(t1 - t0);
        this.checkArray(dataToSort);

        console.log(dataToSort);
        console.log(`Call to doQuickSort took ${t1 - t0} milliseconds.`);

        dataToSort = [...data];
        t0 = performance.now();
        this.doNativeSort(dataToSort);
        t1 = performance.now();
        this.nativeSortData.push(t1 - t0);

        console.log(dataToSort);
        console.log(`Call to doNativeSort took ${t1 - t0} milliseconds.`);
      });
      console.log('insertionSortData', this.insertionSortData);
      console.log('selectionSortData', this.selectionSortData);
      console.log('quickSortData', this.quickSortData);
      console.log('nativeSortData', this.nativeSortData);
    },

    doInsertionSort(arr) {
      for (let i = 1; i < arr.length; i++) {
        if (arr[i] < arr[i - 1]) {
          for (let j = 0; j <= i; j++) {
            if (arr[j] > arr[i]) {
              let itemToMove = arr.splice(i, 1)[0];
              arr.splice(j, 0, itemToMove);
              break;
            }
          }
        }
      }
      return arr;
    },

    doSelectionSort(arr) {
      for (let i = 0; i < arr.length - 1; i++) {
        let minElementIdx = this.findMinElementIndex(arr, i, arr.length);
        this.swapElements(arr, i, minElementIdx);
      }
      return arr;
    },

    findMinElementIndex(arr, beginIdx, endIdx) {
      let minElementIdx = beginIdx;
      for (let j = beginIdx + 1; j <= endIdx; j++) {
        if (arr[minElementIdx] > arr[j]) {
          minElementIdx = j;
        }
      }
      return minElementIdx;
    },

    swapElements(arr, idx1, idx2) {
      if (idx1 != idx2) {
        let tmpValue = arr[idx1];
        arr[idx1] = arr[idx2];
        arr[idx2] = tmpValue;
      }
    },

    doQuickSort(arr) {
      this.quickSort(arr, 0, arr.length - 1);
      // this.sortPartition(arr, 0, arr.length - 1);
      return arr;
    },

    partition(items, left, right) {
      var pivot = items[Math.floor((right + left) / 2)];
      var i = left;
      var j = right;

      while (i <= j) {
        while (items[i] < pivot) {
          i++;
        }
        while (items[j] > pivot) {
          j--;
        }
        if (i <= j) {
          this.swapElements(items, i, j);
          i++;
          j--;
        }
      }
      return i;
    },

    quickSort(items, left, right) {
      var index;
      if (items.length > 1) {
        index = this.partition(items, left, right);
        if (left < index - 1) {
          this.quickSort(items, left, index - 1);
        }
        if (index < right) {
          this.quickSort(items, index, right);
        }
      }
      return items;
    },

    checkArray(arr) {
      for (let i = 1; i < arr.length; i++) {
        if (arr[i] < arr[i - 1]) {
          console.log('ARRAY NOT SORTED');
          return false;
        }
      }
      return true;
    },

    // SORT PARTITION USING SPLICE FOR SWAPPING ELEMENTS IN ARRAY, NOT OPTIMAL WAY, SPLICE IS SLOW

    // sortPartition(arr, start, end) {
    //   // find pivot index
    //   let pivotIndex = Math.round((start + end) / 2);
    //   let newPivotIndex = pivotIndex;
    //   let pivotValue = arr[pivotIndex];

    //   // move all smaller values from right to left partition
    //   for (let i = pivotIndex + 1; i <= end; i++) {
    //     if (arr[i] < pivotValue) {
    //       let itemToMove = arr.splice(i, 1)[0];
    //       arr.splice(pivotIndex, 0, itemToMove);
    //       ++newPivotIndex;
    //     }
    //   }

    //   // move all larger values from left to right partition
    //   for (let i = pivotIndex - 1; i >= start; i--) {
    //     if (arr[i] > pivotValue) {
    //       let itemToMove = arr.splice(i, 1)[0];
    //       arr.splice(end, 0, itemToMove);
    //       --newPivotIndex;
    //     }
    //   }

    //   // sorting left and right partitions with updated pivot index
    //   if (newPivotIndex - 1 - start >= 1) {
    //     this.sortPartition(arr, start, newPivotIndex - 1);
    //   }
    //   if (end - newPivotIndex - 1 >= 1) {
    //     this.sortPartition(arr, newPivotIndex + 1, end);
    //   }
    // },

    // SORT PARTITION USING ADDITIONAL ARRAY, NOT OPTIMAL WAY, REQUIRES MORE MEMORY

    // sortPartition(arr, start, end) {
    //   // find pivot index
    //   let pivotIndex = Math.round((start + end) / 2);
    //   let pivotValue = arr[pivotIndex];

    //   let newArr = [...arr];
    //   let newArrIdx = start;

    //   for (let i = start; i <= end; i++) {
    //     if (arr[i] <= pivotValue && i != pivotIndex) {
    //       newArr[newArrIdx] = arr[i];
    //       ++newArrIdx;
    //     }
    //   }

    //   let newPivotIndex = newArrIdx;
    //   newArr[newPivotIndex] = arr[pivotIndex];
    //   ++newArrIdx;

    //   for (let i = start; i <= end; i++) {
    //     if (arr[i] > pivotValue) {
    //       newArr[newArrIdx] = arr[i];
    //       ++newArrIdx;
    //     }
    //   }

    //   // sorting left and right partitions with updated pivot index
    //   if (newPivotIndex - 1 - start >= 1) {
    //     this.sortPartition(newArr, start, newPivotIndex - 1);
    //   }
    //   if (end - newPivotIndex - 1 >= 1) {
    //     this.sortPartition(newArr, newPivotIndex + 1, end);
    //   }

    //   arr = [...newArr];
    // },
  },
  computed: {
    displayedLabels() {
      return DATA_COUNT.map(count => `${count} items`);
    },
    displayedDatasets() {
      return [
        {
          label: 'Insertion Sort',
          borderColor: 'rgba(32, 156, 238, 1)',
          fill: false,
          data: this.insertionSortData,
        },
        {
          label: 'Selection Sort',
          borderColor: 'rgba(35, 209, 96, 1)',
          fill: false,
          data: this.selectionSortData,
        },
        {
          label: 'Quick Sort',
          borderColor: 'rgba(255, 221, 87, 1)',
          fill: false,
          data: this.quickSortData,
        },
        {
          label: 'Native Sort',
          borderColor: 'rgba(255, 56, 96, 1)',
          fill: false,
          data: this.nativeSortData,
        },
      ];
    },
  },
};
</script>

<style scoped>
.container {
  padding: 2rem 0;
}
.box {
  padding: 1.5rem;
}
</style>
