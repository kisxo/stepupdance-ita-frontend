<script lang="ts">
    import { Dialog, Label, Separator } from "bits-ui";
    import { fade } from "svelte/transition";
    import { flyAndScale } from "$lib/bits-ui/utils/transitions";
    import { goto } from "$app/navigation";
    let { toggle, body, cancel="cancel", redirect=''} = $props();

    function check_redirect(){
      if(redirect != '')
      {
        goto(redirect);
      }
    }
</script>
   
  <Dialog.Root bind:open={toggle} closeOnOutsideClick={false}>
    <Dialog.Portal>
      <Dialog.Overlay transition={fade} transitionConfig={{ duration: 150 }} class="fixed inset-0 z-50 bg-black/80"/>
      <Dialog.Content transition={flyAndScale} class="fixed text-center flex flex-col justify-center gap-[2rem] bg-white border-none rounded-md py-[3rem] left-[50%] top-[50%] z-50 w-full max-w-[94%] translate-x-[-50%] translate-y-[-50%] rounded-card-lg border bg-background p-5 shadow-popover outline-none sm:max-w-[490px] md:w-full">
        <div class="flex flex-row justify-center">
            <svg xmlns="http://www.w3.org/2000/svg" height="50" width="50" class="text-green-500 bg-white rounded-[50%] drop-shadow" viewBox="0 0 256 256">
              <rect width="256" height="256" fill="none"/>
              <polyline points="88 136 112 160 168 104" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="24"/>
            </svg>
        </div>
        <Dialog.Description class="text-sm w-[75%] mx-auto text-foreground-alt">
            {body}
        </Dialog.Description>
        
        <Dialog.Close onclick={check_redirect} class="bg-blue-500 text-white right-5 top-5 rounded-xm mx-auto w-[75%] py-[.5rem] focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-foreground focus-visible:ring-offset-2 focus-visible:ring-offset-background active:scale-98">
            <span>{cancel}</span>
        </Dialog.Close>
      </Dialog.Content>
    </Dialog.Portal>
  </Dialog.Root>