<script lang="ts">
  import { fade, fly } from 'svelte/transition';
  import { tweened } from 'svelte/motion';
  import { cubicOut } from 'svelte/easing';
  import { onMount } from 'svelte';

  // Mantenemos la identidad visual del número gigante de fondo
  const backgroundOdd = tweened(3.45, { duration: 1500 });
  
  // Animación de una cuota en vivo
  const liveOdd = tweened(2.85, { duration: 400, easing: cubicOut });
  let isUp = true;

  onMount(() => {
    const int1 = setInterval(() => {
      backgroundOdd.update(n => Math.max(1.01, n + (Math.random() - 0.5)));
    }, 4000);
    
    const int2 = setInterval(() => {
      const old = $liveOdd;
      const next = old + (Math.random() > 0.5 ? 0.10 : -0.10);
      isUp = next > old;
      liveOdd.set(next);
    }, 3000);

    return () => { clearInterval(int1); clearInterval(int2); };
  });
</script>

<div class="relative min-h-screen bg-[#051329] text-white font-sans overflow-hidden">
  
  <div class="fixed inset-0 flex items-center justify-center pointer-events-none z-0">
    <span class="text-[45vw] font-black tracking-tighter opacity-10 outlined-text">
      {$backgroundOdd.toFixed(1)}
    </span>
  </div>

  <nav class="relative z-10 w-full bg-[#0B1E3B]/80 backdrop-blur-xl border-b border-white/10 px-8 py-4 flex justify-between items-center" in:fly={{ y: -50, duration: 600 }}>
    <div class="flex items-center gap-4">
      <span class="text-3xl font-serif font-bold text-gradient">SB</span>
      <div class="h-8 w-px bg-white/20"></div>
      <span class="font-bold tracking-widest uppercase text-sm text-[#88AACC]">En Vivo</span>
    </div>
    
    <div class="flex items-center gap-6">
      <div class="text-right">
        <p class="text-[10px] text-[#88AACC] uppercase tracking-widest">Balance</p>
        <p class="text-[#F4C430] font-black text-xl">$ 1,250.00</p>
      </div>
      <div class="w-12 h-12 rounded-xl bg-gradient-to-br from-[#112647] to-[#051329] border border-white/10 shadow-inner flex items-center justify-center">
        <span class="text-[#88AACC]">👤</span>
      </div>
    </div>
  </nav>

  <main class="relative z-10 max-w-5xl mx-auto p-8 mt-4">
    <h2 class="text-2xl font-black mb-6 text-white flex items-center gap-3" in:fade={{ delay: 200 }}>
      <span class="w-3 h-3 rounded-full bg-red-500 animate-pulse shadow-[0_0_10px_rgba(239,68,68,0.8)]"></span>
      Destacados Champions League
    </h2>

    <div 
      class="bg-[#0B1E3B]/60 backdrop-blur-md border border-white/10 rounded-[2rem] p-8 shadow-[0_20px_40px_rgba(0,0,0,0.4)] transition-all hover:bg-[#0B1E3B]/80"
      in:fly={{ y: 30, duration: 600, delay: 300 }}
    >
      <div class="flex justify-between items-center border-b border-white/10 pb-4 mb-6">
        <span class="text-[#88AACC] font-bold text-sm tracking-wider uppercase">Minuto 75' • 2da Mitad</span>
        <span class="text-[#F4C430] font-bold text-sm bg-[#F4C430]/10 px-3 py-1 rounded-full">Resultado Final</span>
      </div>

      <div class="flex justify-between items-center gap-4">
        
        <div class="flex-1 text-center bg-[#112647]/50 rounded-2xl p-6 border border-white/5">
          <p class="text-2xl font-black mb-1">Real Madrid</p>
          <p class="text-4xl font-bold text-[#88AACC] mb-4">2</p>
          <button class="w-full py-3 rounded-xl bg-white/5 hover:bg-white/10 border border-white/10 font-bold text-lg transition-colors">
            1.85
          </button>
        </div>

        <div class="w-1/4 text-center">
          <p class="text-[#88AACC] font-bold mb-4">Empate</p>
          <button class="w-full py-3 rounded-xl bg-white/5 hover:bg-white/10 border border-white/10 font-bold text-lg transition-colors">
            3.40
          </button>
        </div>

        <div class="flex-1 text-center bg-[#112647]/50 rounded-2xl p-6 border border-white/5 relative overflow-hidden">
          <div class="absolute inset-0 opacity-20 transition-colors duration-500 {isUp ? 'bg-green-500' : 'bg-red-500'}"></div>
          
          <p class="relative z-10 text-2xl font-black mb-1">Bayern</p>
          <p class="relative z-10 text-4xl font-bold text-[#88AACC] mb-4">2</p>
          <button class="relative z-10 w-full py-3 rounded-xl font-black text-lg transition-colors duration-300 shadow-lg {isUp ? 'bg-green-500 text-white' : 'bg-red-500 text-white'}">
            {$liveOdd.toFixed(2)}
            <span class="text-sm ml-1">{isUp ? '↑' : '↓'}</span>
          </button>
        </div>

      </div>
    </div>
  </main>
</div>

<style>
  .outlined-text {
    color: transparent;
    -webkit-text-stroke: 2px rgba(244,196,48,0.5);
  }
  .text-gradient {
    background: linear-gradient(to bottom, #FFF0A0, #D4A017);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
</style>