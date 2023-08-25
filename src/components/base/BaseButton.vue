<template>
  <button><slot></slot></button>
</template>

<script>
export default {};
</script>

<style lang="scss" scoped>
button {
  width: 120px;
  padding: 2rem 1rem;
  margin: 1rem 0.5rem;
  border: 1px solid var(--light);
  border-radius: 1rem;
  background-color: transparent;
  color: var(--light);
  font-family: var(--font);
  cursor: pointer;
  transition: all 0.3s linear;
  &:hover {
    background-color: var(--light);
    color: var(--dark);
  }
}
</style>
