<script lang="ts">
  let img = $state("sample.png");
  let num = $state(8);
  let files: FileList | null | undefined = $state();

  $effect(() => {
    if (files) {
      // base64エンコード
      const reader = new FileReader();
      reader.readAsDataURL(files[0]);

      reader.onload = () => {
        img = reader.result as string;
      };
    }
  });
</script>

<div class="control-panel">
  <label>
    <span>枚数</span>
    <input type="number" bind:value={num} />
  </label>
  <input type="file" accept="image/*" bind:files />
  <button onclick={() => window.print()}>印刷</button>
  <p>※縦横比1:1.65(55mm x 91mm)推奨</p>
  <p>※このツールはブラウザ内のみで画像を処理します</p>
</div>

<div class="container">
  {#each { length: num } as _}
    <div class="card">
      <img src={img} alt="namecard" />
    </div>
  {/each}
</div>

<style>
  @page {
    /* size: A4; */
    margin: 0;
  }

  .control-panel {
    margin: 10px;
  }

  .container {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    justify-content: space-evenly;
    margin-top: 20px;
  }

  /* 改ページ */
  /* .card:nth-child(10n + 1):not(:nth-child(-n + 10)) {
    page-break-before: always;
    margin-top: 15px;
  }
  .card:nth-child(10n + 2):not(:nth-child(-n + 10)) {
    margin-top: 15px;
  } */

  .card {
    border: 1px solid black;
    box-sizing: border-box;
    height: 55mm;
    /* margin: 6px; */
    overflow: hidden;
    width: 91mm;
  }

  .card > img {
    height: 100%;
    object-fit: cover;
    width: 100%;
  }

  @media print {
    .control-panel {
      display: none;
    }
  }
</style>
