<div class="modal" id="account-modal" tabindex="-1" data-bs-backdrop="static">
  <div class="modal-dialog modal-lg">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="modalLabel">勘定科目</h5>
        <button type="button" class="btn-close" id="close-button" area-label="Close"
            on:click={close_}></button>
      </div>
      <div class="modal-body">
        <AccountEntry
          bind:account={account}
          bind:subAccount={subAccount}
          mode={mode}>
        </AccountEntry>
      </div>
      <div class="modal-footer">
        {#if ( ( mode == 'edit-account' ) || ( mode == 'edit-sub-account') )}
          <!-- button type="button" class="btn btn-danger"
                  id="delete-button">Delete</button -->
        {/if}
        <button type="button" class="btn btn-primary"
            on:click={save}
            id="save-button">保存&nbsp;<i class="bi bi-save"></i></button>
      </div>
    </div>
  </div>
</div>

<script>
import axios from 'axios';
import AccountEntry from './account-entry.svelte';
import {onMount, beforeUpdate, afterUpdate, createEventDispatcher} from 'svelte';
const dispatch = createEventDispatcher();

export	let	status;
export	let	mode;
export	let	modal;
export	let account;
export	let	subAccount;

beforeUpdate(() => {
  console.log('account-modal beforeUpdate');
})

const update_account = (term, mode, params) => {
  let pr;
  console.log('update', term, mode, params)
  switch(mode) {
    case 'edit-account':
      pr = axios.put(`/api/account/${term}`, params);	// OK
      break;
    case 'edit-sub-account':
      pr = axios.put(`/api/sub_account/${term}`, params);	//	OK
      break;
    case 'new-account':
      pr = axios.post(`/api/account/${term}`, params);
      break;
    case 'new-sub-account':
      pr = axios.post(`/api/sub_account/${term}`, params);
      break;
  }
  return	(pr);
}
const	save = async(event) => {
  let name = account.name;
  let	sub_name;
  let	key;
  let	tax_class;
  let	debit;
  let	credit;
  let	balance;
  let klass_id;
  if	( subAccount )	{
    sub_name = subAccount.name;
    key = subAccount.key;
    tax_class = subAccount.taxClass;
    debit = subAccount.debit;
    credit = subAccount.credit;
    balance = subAccount.balance;
  } else {
    key = account.key;
    tax_class = account.taxClass;
    debit = account.debit;
    credit = account.credit;
    balance = account.balance;
    klass_id = account.klass_id;
  }

  console.log('save', name, sub_name, key, tax_class, debit, credit, balance);
console.log({mode});
  update_account(status.term, mode, {
    code: account.code,
    sub_code: subAccount ? subAccount.code : 0,
    name: name,
    sub_name, sub_name,
    key: key,
    tax_class: tax_class,
    debit: debit,
    credit: credit,
    balance: balance,
    klass_id: klass_id
  }).then(() => {
    close_();
  });
}
const	close_ = () => {
  dispatch('close');
  modal.hide();
}
</script>
