<script lang="ts">
  import { fade, fly, scale } from 'svelte/transition';
  import { tweened } from 'svelte/motion';
  import { quintOut, cubicOut } from 'svelte/easing';
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation'; // <-- Aquí está la magia de navegación

  // Animación del número de fondo (solo contorno)
  const backgroundOdd = tweened(18.38, { duration: 1500, easing: quintOut });
  
  onMount(() => {
    const i = setInterval(() => {
      backgroundOdd.update(n => Math.max(1.01, n + (Math.random() - 0.5) * 2));
    }, 3000);
    return () => clearInterval(i);
  });

  let email = 'qwewq@asc.com';
  let password = '••••••••';
  let loading = false;
  let progress = tweened(0, { duration: 2500, easing: cubicOut });

  function handleLogin() {
    loading = true;
    progress.set(100);
    // Después de 2.5 segundos de la barra cargando, saltamos al dashboard
    setTimeout(() => {
      goto('/dashboard');
    }, 2500);
  }
</script>

<div class="relative min-h-screen bg-[#051329] flex items-center justify-center overflow-hidden font-sans">
  
  <div class="absolute top-1/4 left-1/4 w-96 h-96 bg-white/5 rounded-full blur-[100px] pointer-events-none"></div>
  <div class="absolute bottom-1/4 right-1/4 w-96 h-96 bg-[#F4C430]/10 rounded-full blur-[100px] pointer-events-none"></div>

  {#if loading}
    <div 
      class="absolute top-8 left-1/2 -translate-x-1/2 w-80 bg-gradient-to-b from-[#FAD65A] to-[#E5B53A] rounded-xl p-4 shadow-[0_0_30px_rgba(244,196,48,0.3)] z-50 border border-[#FFF0A0]/50"
      in:fly={{ y: -50, duration: 400, easing: quintOut }}
      out:fade
    >
      <div class="text-center text-[#001D41] font-bold text-lg mb-3">¡Iniciando sesión!</div>
      <div class="h-2 w-full bg-white/40 rounded-full overflow-hidden">
        <div class="h-full bg-white rounded-full transition-all" style="width: {$progress}%"></div>
      </div>
    </div>
  {/if}

  <div class="absolute inset-0 flex items-center justify-center pointer-events-none select-none z-0">
    <span class="text-[35vw] font-black tracking-tighter opacity-40 outlined-text">
      {$backgroundOdd.toFixed(2)}
    </span>
  </div>

  <div 
    class="relative z-10 w-full max-w-[380px] p-8 bg-[#0B1E3B]/60 backdrop-blur-xl border border-white/10 rounded-[2rem] shadow-[0_20px_50px_rgba(0,0,0,0.5)]"
    in:scale={{ duration: 600, delay: 100, start: 0.95, easing: cubicOut }}
  >
    <div class="text-center mb-8 relative">
      <div class="flex justify-center items-center gap-4 mb-4">
        <span class="text-4xl font-serif font-bold text-gradient">SB</span>
      </div>
      <h1 class="text-3xl font-extrabold text-white tracking-tight drop-shadow-md">Entra al Juego</h1>
      <p class="text-[#88AACC] mt-1 text-sm font-medium">Las mejores cuotas te esperan.</p>
    </div>

    <form class="space-y-5" on:submit|preventDefault={handleLogin}>
      
      <div class="space-y-1.5">
        <label class="text-[10px] font-bold text-[#88AACC] uppercase tracking-wider ml-1">Correo</label>
        <div class="relative">
          <span class="absolute left-4 top-1/2 -translate-y-1/2 text-[#88AACC]">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"></path></svg>
          </span>
          <input type="email" bind:value={email} class="custom-input" disabled={loading}>
        </div>
      </div>

      <div class="space-y-1.5">
        <label class="text-[10px] font-bold text-[#88AACC] uppercase tracking-wider ml-1">Contraseña</label>
        <div class="relative">
          <span class="absolute left-4 top-1/2 -translate-y-1/2 text-[#88AACC]">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z"></path></svg>
          </span>
          <input type="password" bind:value={password} class="custom-input" disabled={loading}>
        </div>
      </div>

      <button type="submit" class="bet-button" disabled={loading}>
        Apostar Ahora.
      </button>
    </form>
  </div>
</div>

<style>
  .outlined-text {
    color: transparent;
    -webkit-text-stroke: 3px #F4C430;
    filter: drop-shadow(0 0 20px rgba(244,196,48,0.2));
  }
  .text-gradient {
    background: linear-gradient(to bottom, #FFF0A0, #D4A017);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  .custom-input {
    width: 100%;
    background-color: #112647;
    border: 1px solid rgba(255,255,255,0.1);
    color: white;
    border-radius: 0.75rem;
    padding: 0.75rem 1rem 0.75rem 3rem;
    outline: none;
    transition: all 0.3s;
  }
  .custom-input:focus { border-color: rgba(244,196,48,0.5); }
  .bet-button {
    width: 100%;
    margin-top: 2rem;
    background: linear-gradient(to bottom, #FAD65A, #D4A017);
    color: #051329;
    font-weight: 800;
    font-size: 1.125rem;
    border-radius: 0.75rem;
    padding: 0.875rem 1rem;
    box-shadow: 0 5px 15px rgba(244,196,48,0.2);
    transition: transform 0.2s;
  }
  .bet-button:hover { transform: scale(1.02); }
  .bet-button:active { transform: scale(0.98); }
</style>