<script setup>
import { reactive, ref, defineEmits } from 'vue';
import TodoButton from "./TodoButton.vue";

const emit = defineEmits(["create-todo"]);
// input data update
 const todo = ref("");
 const todoState = reactive({
    todo: "",
    invalid: null,
    errMsg: "",

 });

 const createTodo = () => {
    todoState.invalid = null;
    if(todoState.todo !== ""){
        emit("create-todo", todoState.todo);
        todoState.todo = "";
        return
    }
    todoState.invalid = true;
    todoState.errMsg = "Value Cannot be empty";   
 };
//  const todoState = reactive({
//     todo: "testing update"
//  });
</script>

<template>
                  <div class="">
                      <div class="">
                          <div class="d-flex justify-content-center">
                            <div class="input-group w-50 input1" :class="{'input-error' : todoState.invalid}">
                                <input
                                    type="text"
                                    class="form-control" v-model="todoState.todo"
                                    placeholder="Todo Title"
                                    aria-label="Example input"
                                    aria-describedby="button-addon1"
                                />
                              <TodoButton @click = "createTodo()" />
                            </div>
                        
                        </div>
                        <!-- Both does the same thing -->
                        <p v-if="todoState.invalid" class="text-danger small">*{{ todoState.errMsg }}</p>
                        <!-- <p v-show ="todoState.invalid" class="text-danger small">*{{ todoState.errMsg }}</p> -->
                      </div>
                     
                  </div>  
</template>


<style lang="scss" scoped>
.input1{
   display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;
   &.input-error {
    border-color: red;
  }

  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

}

</style>