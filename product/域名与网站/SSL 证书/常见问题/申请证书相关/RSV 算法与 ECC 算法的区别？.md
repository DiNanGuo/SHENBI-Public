### RSA 加密算法与 ECC 加密算法的区别？
- RSA 加密算法：国际标准算法，应用较早的算法之一，普遍性更强，同比 ECC 算法的适用范围更广，兼容性更好，一般采用2048位的加密长度，服务端性能消耗较高。
- ECC 加密算法：椭圆加密算法，新一代算法趋势主流，一般采用256位加密长度（相当于 RSA 3072 位加密强度）更安全，抗攻击型更强，同比 RSA 算法加密速度快，效率更高，服务器资源消耗更低。


您可以通过下表对比项目查看两种加密算法的具体区别：
<table>
<tr>
<th width="15%">对比项目</th>
<th>ECC 加密算法</th>
<th>RSA 加密算法</th>
</tr>
<tr>
<td>密钥长度</td>
<td>256位</td>
<td>2048位</td>
</tr>
<tr>
<td>CPU 占用</td>
<td>较少</td>
<td>较高</td>
</tr>
<tr>
<td>内存占用</td>
<td>较少</td>
<td>较高</td>
</tr>
<tr>
<td>网络消耗</td>
<td>较少</td>
<td>较高</td>
</tr>
<tr>
<td>加密效率</td>
<td>较高</td>
<td>一般</td>
</tr>
<td>抗攻击性</td>
<td>较强</td>
<td>一般</td>
</tr>
<tr>
<td>兼容范围</td>
<td>新版浏览器和操作系统均支持，但存在少数不支持的平台。例如 cPanel</td>
<td>均支持</td>
</tr>
</table>

