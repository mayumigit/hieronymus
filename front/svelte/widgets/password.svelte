<div class="menu">
    <div class="body">
        <form>
            <div class="row mb-2">
                <label for="inputCurrentPassword" class="col-sm-5 col-form-label">
                    現在のパスワード
                </label>
                <div class="col-sm-7">
                    <input type="text" class="form-control" id="inputCurrentPassword"
                        bind:value={currentPassword}>
                </div>
            </div>
            <div class="row mb-2">
                <label for="inputNewPassword" class="col-sm-5 col-form-label">
                    新しいパスワード
                </label>
                <div class="col-sm-7">
                    <input type="text" class="form-control" id="inputNewPassword"
                        bind:value={newPassword}>
                </div>
            </div>
            <div class="row mb-0">
                <label for="inputConfirmPassword" class="col-sm-5 col-form-label">
                    新しいパスワードの再入力
                </label>
                <div class="col-sm-7">
                    <input type="text" class="form-control" id="inputConfirmPassword"
                        bind:value={confirmPassword}>
                </div>
            </div>
        </form>
    </div>
    <div class="footer">
        <button class="btn btn-primary" on:click|preventDefault={updatePassword}>
            パスワード更新
        </button>
    </div>
</div>
<script>
import axios from 'axios';

let currentPassword;
let newPassword;
let confirmPassword;

export let toast;

const updatePassword = (event) => {
    if  ( newPassword ) {
        if  ( newPassword == confirmPassword )    {
            axios.put('/api/user/password', {
                currentPassword: currentPassword,
                newPassword: newPassword
            }).then((res) => {
                toast.show('パスワード', 'パスワードを更新しました');
                currentPassword = '';
                newPassword = '';
                confirmPassword = '';
            }).catch ((e) => {
                toast.show('パスワード', 'パスワードを更新できませんでした');
            });
        } else {
            toast.show('パスワード', 'パスワードが間違っています');
        }
    }
}

</script>