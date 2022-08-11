---
title: '백준 22860번 JAVA : 폴더 정리 (small)'
date: 2022-08-11 18:06:09
category: 'PS'
draft: false
---

# 문제

백준 문제 링크 : https://www.acmicpc.net/problem/22860

# 풀이전략

사실 아직 해결하지 못했다. 나중에 다시 도전해 볼 예정이다.

1. 폴더를 생성할 때 순서대로 들어오지 않는다. (이거 때문에 엄청 걸림)
2. 폴더가 순서대로 들어오지 않기 때문에, 폴더를 생성할 때, 폴더를 재배치 해주어야한다.
3. 파일을 생성할 때, 하위 디렉토리에 이미 해당 파일이 존재하는지(duplicatedFile), 우리가 생성한 파일이 존재하는지(wantedFile) 확인하며 파일을 생성한다.

# 코드

```java


package backjoon.P22860;

import java.io.*;
import java.util.*;



// MAIN 이 곡 제일먼저 나온다는 문제의 조건은 없음
// 이걸 고려해서 Node를 만들어야 함
// File 과 Folder 가 들어오는 것은 랜덤

// file 들은 상관없음
// folder 만 순서대로 배열 해야함....

public class Main {

    static int N;
    static int M;

    static class Node {
        String name;
        int typeCnt;
        int fileCnt;
        ArrayList<Node> folders;
        HashMap<String, Boolean> files;

        public Node(String name, int typeCnt, int fileCnt) {
            this.name = name;
            this.typeCnt = typeCnt;
            this.fileCnt = fileCnt;
            folders = new ArrayList<>();
            files = new HashMap<>();
        }

        public Node(String name, ArrayList<Node> childes){
            this.name = name;
            this.typeCnt = 0;
            this.fileCnt = 0;
            folders = childes;
            files = new HashMap<>();
        }
    }

    static ArrayList<String> fileStatements = new ArrayList<>();
    static ArrayList<String> folderStatements = new ArrayList<>();




    // 1일때는 원하는 파일이 아래에 존재함
    // 2일떄는 중복된 파일이 존재함
    // 3일때는 둘다 존재

    public static int addFile(Node cur, String folder, String filename) {
        boolean wantedFile = false;
        boolean duplicatedFile = cur.files.containsKey(filename);

        if (Objects.equals(cur.name, folder)) {
            cur.files.put(filename, true);
            wantedFile = true;
        }

        for (int i = 0; i < cur.folders.size(); i++) {
            int val = addFile(cur.folders.get(i), folder, filename);
            if (val == 3) {
                wantedFile = true;
                duplicatedFile = true;
            } else if (val == 2)
                duplicatedFile = true;
            else if (val == 1)
                wantedFile = true;
        }
        if (wantedFile && duplicatedFile) {
            cur.fileCnt++;
            return 3;
        } else if (wantedFile) {
            cur.fileCnt++;
            cur.typeCnt++;
            return 1;
        } else if (duplicatedFile)
            return 2;
        return 0;
    }

    public static void findQuery(Node cur, String[] folders, int idx) {
        if (idx >= folders.length)
            return;

        if (idx == folders.length - 1) {
            System.out.println(cur.typeCnt + " " + cur.fileCnt);
            return;
        }


        cur.folders.forEach(folder -> {
            if (Objects.equals(folder.name, folders[idx + 1])) {
                findQuery(folder, folders, idx + 1);
            }
        });

    }

    public static void findAndCreateNode(Node cur, String folder, String name) {

        if (Objects.equals(cur.name, folder)) {

            cur.folders.add(new Node(name, 0, 0));
            return;
        }
        for (int i = 0; i < cur.folders.size(); i++) {

            findAndCreateNode(cur.folders.get(i), folder, name);
        }
    }

    public static void print(Node cur, String parent) {
        System.out.println("parent: " + parent + " cur: " + cur.name + " typeCnt: " + cur.typeCnt + " fileCnt: " + cur.fileCnt );
        cur.folders.forEach(folder -> {
            print(folder, cur.name);
        });
    }

    public static void createDirectories(Node root){
        HashMap<String, ArrayList<Node>> nodes = new HashMap<>();
        HashMap<String, Boolean> nodeAppear = new HashMap<>();
        nodes.put("main", new ArrayList<>());
        nodeAppear.put("main", true);
        for(String folderStatement :folderStatements){
            String parent = folderStatement.split(" ")[0];
            String name = folderStatement.split(" ")[1];

            // 부모의 자식 노드가 같이 nodes 에 존재 할 때
            if(nodes.containsKey(parent) && nodes.containsKey(name)){
                nodes.get(parent).add(new Node(name, nodes.get(name)));
                nodes.remove(name);
            }
            // 부모만 nodes 에 존재할 때
            else if(nodes.containsKey(parent)){
                nodeAppear.put(name, true);
                nodes.get(parent).add(new Node(name, 0, 0));
            }
            // 부모가 다른 부모에게 편입된 상태일 때
            else if(nodeAppear.containsKey(parent)){
                nodeAppear.put(name, true);
                // 오류해결
                nodes.forEach((key, value) -> value.forEach(node -> {
                    findAndCreateNode(node, parent, name);
                }));
            }
            else if(nodes.containsKey(name)){
                nodeAppear.put(parent, true);
                nodes.put(parent, new ArrayList<>());
                nodes.get(parent).add(new Node(name, nodes.get(name)));
                nodes.remove(name);
            }
            // 완전 처음나온 부모일 때
            else {
                nodes.put(parent, new ArrayList<>());
                nodes.get(parent).add(new Node(name, 0, 0));
                nodeAppear.put(name, true);
                nodeAppear.put(parent, true);
            }
            root.folders = nodes.get("main");
        }

        for(String fileStatement : fileStatements ){
            String parent = fileStatement.split(" ")[0];
            String name = fileStatement.split(" ")[1];
            addFile(root, parent, name);
        }
    }


    public static void main(String[] args) throws Exception {
        System.setIn(new FileInputStream("src/backjoon/input.txt"));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st;

        st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());

        Node root = new Node("main", 0, 0);

        for (int i = 0; i < N + M; i++) {
            st = new StringTokenizer(br.readLine());
            String folder = st.nextToken();
            String name = st.nextToken();
            boolean isFolder = Integer.parseInt(st.nextToken()) == 1;



            if (isFolder)
                folderStatements.add(folder + " " + name);
            else
                fileStatements.add(folder + " " + name);
        }
        createDirectories(root);
        print(root, " ");
        int T = Integer.parseInt(br.readLine());

        for (int t = 0; t < T; t++) {
            String[] sArr = br.readLine().split("/");
            findQuery(root, sArr, 0);
        }

        bw.flush();
        bw.close();
        br.close();
    }
}



```

# 회고

해결하지 못함.... 나중에 다시 도전한다.
