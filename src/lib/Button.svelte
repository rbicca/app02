<script>
    //    @use '../../styles/variables.scss';    
    export let size = 'small';
    export let shadow = false;
    export let bgColor = undefined;
    export let textColor = undefined;
    export let disabled = false;

    //Com $$slots conseguimos descobrir quais os slots que foram passados

    let isEsquerdo;

</script>

<button 
    on:click
    disabled =  {disabled}
    style:background-color={bgColor}
    style:color={textColor}
    class:size-lg={size === 'large'}
    class:size-sm={size === 'small'}
    class:shadow
    >
   
    <!-- Ou fazer o override da variavel scss -->
    <!-- style:--buttonBgColor={bgColor} -->
    <!-- style:--buttonTextColor={textColor} -->

    <div class="left-content" on:mouseenter={() => isEsquerdo = true} on:mouseleave={() => isEsquerdo = false} >
        <slot name="leftContent"/>
    </div>
    <slot {isEsquerdo}>Vazio</slot>
</button>


<style lang="scss">
   button {
    display: flex;
    align-items: center;
    border: none;
    background-color: var(--buttonBgColor);
    color: var(--buttonTextColor);
    
    font-weight: bold;
    border-radius: 5px;
    cursor: pointer;
    .left-content {
        margin-right: 5px;
        max-width: 20px;
        max-height: 20px;
    }
    &:hover {
        // background-color: variables.$color;
        background-image: linear-gradient(rgba(0,0,0,0.4) 0 0);
    }
    &:active {
        background-image: linear-gradient(rgba(255,255,255,0.1) 0 0);
    }
    &.size-sm {
        padding: 10px 15px;
    }
    &.size-lg {
        padding: 20px 25px;
        font-size: 20px;
    }
    &.shadow {
        box-shadow: 0 0 30px rgba(86, 18, 195, 0.3);
    }
    &.disabled {
        background-color: gray;
    }
   } 
</style>