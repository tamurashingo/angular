<!---
# Pipes
--->
# パイプ

<!---
Pipes can be appended on the end of the expressions to translate the value to a different format. Typically used
to control the stringification of numbers, dates, and other data, but can also be used for ordering, mapping, and 
reducing arrays. Pipes can be chained.
--->

パイプは、値を違う形式に変換するために式の最後に追加することができます。
一般的に、数値、データ、そのほかのデータを文字にするために使われますが、
配列の順位付け、map、recudeにも使うことができます。
パイプは連鎖させることができます。


<!---
NOTE: Pipes are known as filters in Angular v1.
--->

注: パイプはAngular v1ではフィルタとして知られている

<!---
Pipes syntax is:
--->
パイプの構文:

```
<div class="movie-copy">
  <p>
    {{ model.getValue('copy') | async | uppercase }}
  </p>
  <ul>
    <li>
      <b>Starring</b>: {{ model.getValue('starring') | async }}
    </li>
    <li>
      <b>Genres</b>: {{ model.getValue('genres') | async }}
    </li>
  <ul>
</div>
```


