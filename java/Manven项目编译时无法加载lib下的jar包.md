maven项目编译的时候，报 `xxxxx 包找不到` ，但是这些包已经放在 `lib` 目录下了，只不过不是 `maven` 中的依赖包，解决方法：

`pom.xml` 文件中加入：

		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-compiler-plugin</artifactId>
			<version>3.6.1</version>
			<configuration>
				<source>1.8</source>
				<target>1.8</target>
				<compilerArguments>
                    <extdirs>${project.basedir}/src/main/webapp/WEB-INF/lib</extdirs>
                </compilerArguments>
			</configuration>
		</plugin>