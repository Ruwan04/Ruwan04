package btc.btc2;

import java.io.FileWriter;
import java.io.PrintWriter;
import java.util.Scanner;

import org.bitcoinj.core.Address;
import org.bitcoinj.core.DumpedPrivateKey;
import org.bitcoinj.core.ECKey;
import org.bitcoinj.core.NetworkParameters;

public class Btc {
    public static void main(String[] args) throws Exception {
        Scanner reader = new Scanner(System.in);
        System.out.println("Enter Bitcoin Address: ");
        String givenAddress = reader.nextLine();
        PrintWriter writer = new PrintWriter(new FileWriter("foundaddress.txt"));

        String net = "prod";
        if (args.length >= 1 && ("test".equals(args[0]) || "prod".equals(args[0]))) {
            net = args[0];
            System.out.println("Using " + net + " network.");
        }

        final NetworkParameters netParams;
        if ("prod".equals(net)) {
            netParams = NetworkParameters.prodNet();
        } else {
            netParams = NetworkParameters.testNet();
        }

        for (;;) {
            ECKey key = new ECKey();
            Address addressFromKey = key.toAddress(netParams);
            String privateKey = key.getPrivateKeyAsHex();
            DumpedPrivateKey privateKey2 = key.getPrivateKeyEncoded(netParams);

            if (givenAddress.equals(addressFromKey.toString())) {
                writer.print(addressFromKey + " " + privateKey2);
                writer.flush();
                System.out.println("Using " + net + " network, Generated address:\n" + addressFromKey + " da private keyz: " + privateKey + " " + privateKey2);
                break;
            }
        }

        reader.close();
        writer.flush();
        writer.close();
    }
}
