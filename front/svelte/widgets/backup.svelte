<div class="menu">
  <div class="header">
    <button class="btn btn-primary" on:click|preventDefault={backup}>
      バックアップ
    </button>
  </div>
  <div class="body">
    <table class="table table-bordered">
      <thead class="table-light">
        <th>
          取得日時
        </th>
        <th>
          処理
        </th>
      </thead>
      <tbody>
        {#each files as file, i}
        <tr>
          <td style="vertical-align:middle;font-size:12pt;">
            {fileName(file)}
          </td>
          <td style="text-align:center;">
            {#if (i == 0) }
            <btton class="btn btn-success" on:click|preventDefault={() => restore(i)}>
              復元
            </btton>
            {:else}
            <btton class="btn btn-warning" on:click|preventDefault={() => restore(i)}>
              復元
            </btton>
            {/if}
            <btton class="btn btn-danger" on:click|preventDefault={() => remove(i)}>
              削除
            </btton>
          </td>
        </tr>
        {/each}
      </tbody>
    </table>
  </div>
</div>
<OkModal
  bind:this={modal}
  title={title}
  description={description}
  on:answer={operation}
  ></OkModal>

<script>
import axios from 'axios';
import {onMount, beforeUpdate} from 'svelte';
import eventBus from '../../javascripts/event-bus.js';
import OkModal from '../common/ok-modal.svelte';

export let toast;
export let status;

let files = [];
let modal;
let description;
let title;
let operation;
let restoreFile;
let removeFile;

const fileName = (file) => {
  return  `${file.getFullYear()}年${file.getMonth()+1}月${file.getDate()}日${file.toLocaleTimeString()}`
}

const remove = (i) => {
  console.log('remove');
  removeFile = files[i];
  if  ( i > 0 ) {
    description = `${fileName(removeFile)}に取得した<br />${i}世代前のバックアップを削除します。<br />よろしいですか？`;
  } else {
    description = `${fileName(removeFile)}に取得した<br />バックアップを削除します。<br />よろしいですか？`;
  }
  title = 'バックアップの削除';
  operation = doRemove;
  modal.show();
}
const restore = (i) => {
  console.log('restore');
  restoreFile = files[i];
  if  ( i > 0 ) {
    description = `${fileName(restoreFile)}に取得した<br />${i}世代前のバックアップから復元します。<br />よろしいですか？`;
  } else {
    description = `${fileName(restoreFile)}に取得した<br />バックアップから復元します。<br />よろしいですか？`;
  }
  title = 'バックアップの復元';
  operation = doRestore;
  modal.show();
}
const doRestore = (ev) => {
  console.log(ev.detail);
  if  ( ev.detail ) {
    console.log('Yes');
    toast.show('バックアップ', '復元開始しました');
    axios.post('/api/admin/restore', {
      date: restoreFile
    }).then((result) => {
      let data = result.data;
      if  ( data.code === 0 ) {
        window.location = '/home';
        toast.remove();
        toast.show('バックアップ', '復元完了しました');
      }
    })
  }
}
const doRemove = (ev) => {
  console.log(ev.detail);
  if  ( ev.detail ) {
    console.log('Yes');
    axios.delete(`/api/admin/backup/${removeFile.toJSON()}`).then(() => {
      toast.show('バックアップ', 'バックアップ削除しました')
      files = undefined;
    })
  }
}

const backup = () => {
  toast.show('バックアップ', 'バックアップ開始しました')
  axios.post('/api/admin/backup').then(() => {
    toast.remove();
    toast.show('バックアップ', 'バックアップ終了しました')
    files = undefined;
  })
}
beforeUpdate(()=> {
  if  ( !files )  {
    axios.get('/api/admin/backups').then((result) => {
      files = [];
      for ( let m of result.data )  {
        files.push(new Date(m));
      }
      files = files;
    })
  }
})

onMount(()=> {
  axios.get('/api/admin/backups').then((result) => {
    console.log('backup', {result});
    files = [];
    for ( let m of result.data )  {
      files.push(new Date(m));
    }
  })
})

</script>