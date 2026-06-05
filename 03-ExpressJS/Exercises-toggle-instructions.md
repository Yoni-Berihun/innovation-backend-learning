Add this HTML `<script>` snippet to the end of `Exercises.md` (paste just before the end of the file). It will change every `<summary>` to "Show Answer" and toggle to "Hide Answer" when opened.

<script>
(function(){
  function initToggle() {
    document.querySelectorAll('details').forEach(det => {
      const summary = det.querySelector('summary');
      if (!summary) return;
      const update = () => { summary.textContent = det.open ? 'Hide Answer' : 'Show Answer'; };
      update();
      det.addEventListener('toggle', update);
    });
  }
  if (typeof document !== 'undefined') {
    if (document.readyState === 'loading') {
      document.addEventListener('DOMContentLoaded', initToggle);
    } else initToggle();
  }
})();
</script>

If you want me to re-run the patch to edit `Exercises.md` directly, I can try again, or you can paste the snippet yourself and save the file. If the Markdown renderer strips `<script>` tags, an alternative is to replace each `<summary>` label with "Show Answer" (I can automate that if you prefer).