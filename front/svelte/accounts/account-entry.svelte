<div class="container-fluid">
    <div class="row">
        <div class="col-3">
            科目クラス
        </div>
        <div class="col-9">
            <div class="row">
                <div class="col-4">大分類</div>
                <div class="col-4">中分類</div>
                <div class="col-4">小分類</div>
            </div>
            <div class="row">
                <div class="col-4">{account.major_name}</div>
                <div class="col-4">{account.middle_name}</div>
                <div class="col-4">{account.minor_name}</div>
            </div>
        </div>
    </div>
    <div class="row">
        <div class="col-3">勘定科目</div>
        <div class="col-9">
            {#if (subAccount)}
                {account.name}
            {:else}
                <input type="text" id="account_name"
                        bind:value={account.name} size="24">
            {/if}
            {#if ( mode == 'new-account') }
                <input type="text" id="account_code"
                    bind:value={account.code} size="24">
            {:else}
                {account.code}
            {/if}
        </div>
    </div>
    {#if ( ( mode == 'new-sub-account' ) || ( mode == 'edit-sub-account' ) )}
    <div class="row">
        <div class="col-3">補助科目</div>
        <div class="col-9">
            <input type="text" id="sub_account_name"
                    bind:value={subAccount.name} size="36">
        </div>
    </div>
    {/if}
    <div class="row">
        <div class="col-3">検索キー</div>
        <div class="col-9">
            {#if ( ( mode == 'new-sub-account' ) || ( mode == 'edit-sub-account' ) )}
            <input type="text" id="key" bind:value={subAccount.key} size="24">
            {:else}
            <input type="text" id="key" bind:value={account.key} size="24">
            {/if}
        </div>
    </div>
    <div class="row">
        <div class="col-3">消費税課税区分</div>
        <div class="col-9">
            {#if ( ( mode == 'new-sub-account' ) || ( mode == 'edit-sub-account' ) )}
            <select class="form-control" id="tax_class" bind:value={subAccount.taxClass}>
                {#each TAX_CLASS as tax_class}
                <option value={tax_class[1]}>{tax_class[0]}</option>
                {/each}
            </select>
            {:else}
            <select class="form-control" id="tax_class" bind:value={account.taxClass}>
                {#each TAX_CLASS as tax_class}
                <option value={tax_class[1]}>{tax_class[0]}</option>
                {/each}
            </select>
            {/if}
        </div>
    </div>
    <div class="row">
        <div class="col-3">借方金額</div>
        <div class="col-9">
            {#if ( ( mode == 'new-sub-account' ) || ( mode == 'edit-sub-account' ) )}
            <InputNumber
                    bind:value={subAccount.debit}></InputNumber>
            {:else}
            <InputNumber
                    bind:value={account.debit}></InputNumber>
            {/if}
        </div>
    </div>
    <div class="row">
        <div class="col-3">貸方金額</div>
        <div class="col-9">
            {#if ( ( mode == 'new-sub-account' ) || ( mode == 'edit-sub-account' ) )}
            <InputNumber
                    bind:value={subAccount.credit}></InputNumber>
            {:else}
            <InputNumber
                    bind:value={account.credit}></InputNumber>
            {/if}
        </div>
    </div>
    <div class="row">
        <div class="col-3">残高</div>
        <div class="col-9">
            {#if ( ( mode == 'new-sub-account' ) || ( mode == 'edit-sub-account' ) )}
            <InputNumber
                    bind:value={subAccount.balance}></InputNumber>
            {:else}
            <InputNumber
                    bind:value={account.balance}></InputNumber>
            {/if}
        </div>
    </div>
</div>

<script>
import {onMount, beforeUpdate, afterUpdate, createEventDispatcher} from 'svelte';
import  InputNumber from '../components/input-number.svelte';
import {TAX_CLASS} from '../../../libs/utils';

export  let account;
export  let subAccount;
export  let mode;

beforeUpdate(() => {
    console.log({account});
    console.log({subAccount});
})
</script>
