# Cn
[3/8, 10:02 PM] Nithish V: //its me.
package bellmanford;

import java.util.Scanner;

public class BellmanFord {

    private int dist[];
    private int n;
    public static final int MAX_VALUE = 999;

    public BellmanFord(int n) {
        this.n = n;
        dist = new int[n + 1];
    }

    public void BellmanFordEvaluation(int src, int adj[][]) {
        for (int nd = 1; nd <= n; nd++) {
            dist[nd] = MAX_VALUE;
        }
        dist[src] = 0;
        for (int nd = 1; nd <= n-1; nd++) {
            for (int sn = 1; sn <= n; sn++) {
                for (int dn = 1; dn <= n; dn++) {
                    if (adj[sn][dn] != MAX_VALUE) {
                        if (dist[dn] > dist[sn] + adj[sn][dn]) {
                            dist[dn] = dist[sn] + adj[sn][dn];
                        }
                    }
                }
            }

            for (int sn = 1; sn <= n; sn++) {
                for (int dn = 1; dn <= n; dn++) {
                    if (adj[sn][dn] != MAX_VALUE) {
                        if (dist[dn] > dist[sn] + adj[sn][dn]) {
                            System.out.println("the graph contains negative edge cycle");
                        }
                    }
                }
            } 
        }
            for (int vertex = 1; vertex <= n; vertex++) {
                System.out.println("Distance of source " + src + " to " + vertex + " is " + dist[vertex]);
            }
        }

    public static void main(String[] args) {

        int n;
        int src;
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter the number of vertices: ");
        n = sc.nextInt();

        int adj[][] = new int[n + 1][n + 1];
        System.out.println("Enter the adjacency matrix: ");
        for (int sn = 1; sn <= n; sn++) {
            for (int dn = 1; dn <= n; dn++) {
                adj[sn][dn] = sc.nextInt();
                if (sn == dn) {
                    adj[sn][dn] = 0;
                    continue;
                }
                if (adj[sn][dn] == 0) {
                    adj[sn][dn] = MAX_VALUE;
                }
            }
        }
        System.out.println("Enter the source vertex: ");
        src = sc.nextInt();

        BellmanFord bellmanford = new BellmanFord(n);
        bellmanford.BellmanFordEvaluation(src, adj);
    }
}


/* 
output:
run:
Enter the number of vertices: 
5
Enter the adjacency matrix: 
0 2 1 99 99 
2 0 1 3 8
1 1 0 2 4
99 3 2 0 3
99 99 4 3 0

Enter the source vertex: 
1
Distance of source 1 to 1 is 0
Distance of source 1 to 2 is 2
Distance of source 1 to 3 is 1
Distance of source 1 to 4 is 3
Distance of source 1 to 5 is 5


*/
[3/8, 10:02 PM] Nithish V: package crc;

import java.util.*;

public class Crc {

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int n;
        System.out.println("Enter The size of the data ");
        n = scan.nextInt();
        int data[] = new int[n];
        System.out.println("Enter The data ,bit by bit");
        for (int i = 0; i < n; i++) {
            data[i] = scan.nextInt();
        }
        System.out.println("Enter the size of Divisor");
        n = scan.nextInt();
        int divisor[] = new int[n];
        System.out.println("Enter The Divisor ,bit by bit ");
        for (int i = 0; i < n; i++) {
            divisor[i] = scan.nextInt();
        }
        int remainder[] = divide(data, divisor);
        System.out.println("remainder=");
        for (int i = 0; i < remainder.length - 1; i++) {
            System.out.println(remainder[i]);
        }
        System.out.println("\n The CRC code generated id:");
        for (int i = 0; i < data.length - 1; i++) {
            System.out.println(data[i]);
        }
        for (int i = 0; i < remainder.length - 1; i++) {
            System.out.println(remainder[i]);
        }
        System.out.println();
        int sent_data[] = new int[data.length + remainder.length - 1];
        System.out.println("Enter The data recieved at reciever:");
        for (int i = 0; i < sent_data.length; i++) {
            sent_data[i] = scan.nextInt();
        }
        receive(sent_data, divisor);
    }

    static int[] divide(int old_data[], int divisor[]) {
        int remainder[], i;
        int data[] = new int[old_data.length + divisor.length];
        System.arraycopy(old_data, 0, data, 0, old_data.length);
        remainder = new int[divisor.length];
        System.arraycopy(data, 0, remainder, 0, divisor.length);
        for (i = 0; i < old_data.length; i++) {
            if (remainder[0] == 1) {
                for (int j = 1; j < divisor.length; j++) {
                    remainder[j - 1] = exor(remainder[j], divisor[j]);
                }
            } else {
                for (int j = 1; j < divisor.length; j++) {
                    remainder[j - 1] = exor(remainder[j], 0);
                }
            }
            remainder[divisor.length - 1] = data[i + divisor.length];
        }
        return remainder;
    }

    static int exor(int a, int b) {
        if (a == b) {
            return 0;
        }
        return 1;
    }

    static void receive(int data[], int divisor[]) {
        int remainder[] = divide(data, divisor);
        for (int i = 0; i < remainder.length; i++) {
            if (remainder[i] != 0) {
                System.out.println("There is an error in received data");
                return;
            }
        }
        System.out.println("Data usal received without any error");
    }
}

[3/8, 10:02 PM] Nithish V: package com.mycompany.leakybucket;

import java.util.Scanner;

public class LeakyBucket {

    public static void main(String[] args) throws InterruptedException {
        int incoming, outgoing, buck_size, n, store = 0;
        Scanner Sc = new Scanner(System.in);

        System.out.println("Enter bucket size, outgoing rate,no of inputs and incoming size: ");
        buck_size = Sc.nextInt();
        outgoing = Sc.nextInt();
        n = Sc.nextInt();

        while (n != 0) {
            System.out.println("Enter the incoming packet size : ");
            incoming = Sc.nextInt();
            if (incoming <= (buck_size - store)) {
                store += incoming;
                System.out.println("Bucket buffer size is " + store + "out of " + buck_size);
            } else {
                System.out.println("Packet Loss " + (incoming - (buck_size - store)));
                store = buck_size;
                System.out.println("Bucket buffer size is " + store + "out of " + buck_size);

            }
            store = store - outgoing;
            System.out.println("After outgoing : " + store + "packets left out of " + buck_size + "in buffer");
            n--;

            Thread.sleep(3000);

        }

    }

}
