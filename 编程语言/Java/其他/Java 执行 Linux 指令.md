# Java 执行 Linux 指令

```java
    /**
     * java 执行 linux 指令方法 executeNewFlow(Collections.singletonList("cd /home")); 通过这种方式调用指令
     */
    public static void executeNewFlow(List<String> commands) throws IOException, InterruptedException {
        List<String> rspList = new ArrayList<>();
        Runtime run = Runtime.getRuntime();
        Process proc = null;
        BufferedReader in = null;
        PrintWriter out = null;
        try {
            proc = run.exec("/bin/bash", null, null);
            in = new BufferedReader(new InputStreamReader(proc.getInputStream()));
            out = new PrintWriter(new BufferedWriter(new OutputStreamWriter(proc.getOutputStream())), true);
            commands.forEach(out::println);
            // 这个命令必须执行，否则in流不结束。
            out.println("exit");
            in.lines().forEach(rspList::add);
        } catch (IOException e1) {
            e1.printStackTrace();
        } finally {
            Objects.requireNonNull(proc).waitFor();
            Objects.requireNonNull(in).close();
            Objects.requireNonNull(out).close();
            proc.destroy();
        }
    }
```

‍
