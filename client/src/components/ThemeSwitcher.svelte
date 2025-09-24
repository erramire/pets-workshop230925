<script lang="ts">
  import { onMount } from 'svelte';
  
  let currentTheme: string = 'dark';
  
  const themes = {
    dark: {
      name: 'Dark Mode',
      icon: '🌙'
    },
    lightBlue: {
      name: 'Light Blue',
      icon: '🌊'
    }
  };
  
  const applyTheme = (theme: string) => {
    const html = document.documentElement;
    const body = document.body;
    
    // Remove all theme classes
    html.classList.remove('dark', 'light-blue');
    body.classList.remove(
      'bg-slate-900', 'text-white',
      'bg-blue-50', 'text-blue-900'
    );
    
    if (theme === 'dark') {
      html.classList.add('dark');
      body.classList.add('bg-slate-900', 'text-white');
    } else if (theme === 'lightBlue') {
      html.classList.add('light-blue');
      body.classList.add('bg-blue-50', 'text-blue-900');
    }
    
    currentTheme = theme;
    localStorage.setItem('theme', theme);
  };
  
  const switchTheme = () => {
    const newTheme = currentTheme === 'dark' ? 'lightBlue' : 'dark';
    applyTheme(newTheme);
  };
  
  onMount(() => {
    // Get saved theme or default to dark
    const savedTheme = localStorage.getItem('theme') || 'dark';
    applyTheme(savedTheme);
  });
</script>

<div class="flex items-center">
  <button
    on:click={switchTheme}
    class="theme-switcher-btn flex items-center space-x-2 px-3 py-2 rounded-lg transition-colors duration-200"
    title="Switch theme"
  >
    <span class="text-lg">{themes[currentTheme as keyof typeof themes].icon}</span>
    <span class="text-sm font-medium hidden sm:block">
      {themes[currentTheme as keyof typeof themes].name}
    </span>
  </button>
</div>

<style>
  /* Theme switcher button styles */
  :global(.dark) .theme-switcher-btn {
    background-color: rgb(51 65 85); /* bg-slate-700 */
    color: white;
  }
  
  :global(.dark) .theme-switcher-btn:hover {
    background-color: rgb(71 85 105); /* bg-slate-600 */
  }
  
  :global(.light-blue) .theme-switcher-btn {
    background-color: rgb(191 219 254); /* bg-blue-200 */
    color: rgb(30 58 138); /* text-blue-800 */
  }
  
  :global(.light-blue) .theme-switcher-btn:hover {
    background-color: rgb(147 197 253); /* bg-blue-300 */
  }
</style>