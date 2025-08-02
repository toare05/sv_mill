<script>
  let score = 0;
</script>

<form on:submit|preventDefault={(e) => {
  score = [...e.target.elements].reduce((acc, el) => {
    if (el.type === 'checkbox' && el.checked) acc += 1;
    return acc;
  }, 0);
}}>
  <label>
    <input type="checkbox" /> 1
  </label>
  <label>
    <input type="checkbox" /> 2
  </label>
  <label>
    <input type="checkbox" /> 3
  </label>
  <button type="submit">점수 확인</button>
</form>

<p>당신의 점수는 {score}점 입니다.</p>
